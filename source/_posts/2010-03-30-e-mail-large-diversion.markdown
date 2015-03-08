---
author: hesicong
comments: true
date: 2010-03-30 10:37:29+00:00
layout: post
slug: e-mail-large-diversion
title: 邮箱大挪移
wordpress_id: 2196
categories:
- 软件
tags:
- imap
- python
---

在GOOGLE被“驱逐”中国后，我越来越担心Gmail的命运了。这几天貌似google.com.hk都间歇性的刷不出来了。就怕哪天完全被屏蔽了，这下啥都完了。目前我的邮箱已经是Google的域名邮箱，里面存有几千个邮件，包括163和以前Gmail的几个邮箱的。但挪移邮箱并不是那么简单的事情，用POP3和SMTP没法完成这个任务。还好IMAP提供了邮件下载和上传的功能，圆满解决我的需求。完整的IMAP协议见：[http://www.faqs.org/rfcs/rfc3501.html](http://www.faqs.org/rfcs/rfc3501.html)

在所有的库中，支持IMAP最好的是Python。Python的imaplib提供了IMAP的很好的支持，而且应用简单([http://docs.python.org/library/imaplib.html](http://docs.python.org/library/imaplib.html))。在IMAP下载中，这个python脚本提供了一个很好的例子：[http://the.taoofmac.com/media/Projects/imapbackup/imapbackup.py.txt](http://the.taoofmac.com/media/Projects/imapbackup/imapbackup.py.txt)

我的需求比较简单，将几个邮箱的数据全部转移到QQ邮箱即可，并且要区分收件箱和发件箱。

实现起来，首先将所有的邮箱转发到需要转移到的邮箱(避免下载的时候多出几个新邮件来)，然后通过IMAP将所有邮箱的数据下载下来(注意有可能有以前通过POP3下载的重复的数据)，最后再上传到QQ邮箱。

具体实现中，将IMAP的数据下载后，对BODY部分的MD5编码作为文件名，然后放置到一个文件夹中，命名为receive。对于制定的发件箱，需要存到sent文件夹中。这样receive中就有所有的邮件，并且不重复。

然后上传到QQ邮箱，上传成功就删除一个本地文件，避免重复上传。

我参考imapbackup.py写了一个IMAPServer类，用于下载和上传

``` python
import os, imaplib, re, hashlib, shutil, getopt

class IMAPServer:
    def __init__(self, server, port,  ssl, usr, psd):
        if ssl:
            self.__server=imaplib.IMAP4_SSL(server, port)
        else:
            self.__server=imaplib.IMAP4(server, port)
        self.__server.debug=4
        self.__server.login(usr, psd)
        print usr , " loged in"

    def __parse_paren_list(self, row):
        """Parses the nested list of attributes at the start of a LIST response"""
        # eat starting paren
        assert(row[0] == '(' )
        row = row[1:]

        result = []

        # NOTE: RFC3501 doesn't fully define the format of name attributes
        name_attrib_re = re.compile("^s*(\\[a-zA-Z0-9_]+)s*")

        # eat name attributes until ending paren
        while row[0] != ')':
            # recurse
            if row[0] == '(':
                paren_list, row = parse_paren_list(row)
                result.append(paren_list)
            # consume name attribute
            else:
                match = name_attrib_re.search(row)
                assert(match != None)
                name_attrib = row[match.start():match.end()]
                row = row[match.end():]
                #print "MATCHED '%s' '%s'" % (name_attrib, row)
                name_attrib = name_attrib.strip()
                result.append(name_attrib)

        # eat ending paren
        assert(')' == row[0])
        row = row[1:]

        # done!
        return result, row

    def __parse_string_list(self, row):
        """Parses the quoted and unquoted strings at the end of a LIST response"""
        slist = re.compile('s*(?:"([^"]+)")s*|s*(S+)s*').split(row)
        return [s for s in slist if s]

    def __parse_list(self, row):
        """Prases response of LIST command into a list"""
        row = row.strip()
        paren_list, row = self.__parse_paren_list(row)
        string_list = self.__parse_string_list(row)
        assert(len(string_list) == 2)
        return [paren_list] + string_list

    def __get_hierarchy_delimiter(self):
        """Queries the imapd for the hierarchy delimiter, eg. '.' in INBOX.Sent"""
        # see RFC 3501 page 39 paragraph 4
        typ, data = self.__server.list('', '')
        assert(typ == 'OK')
        assert(len(data) == 1)
        lst = self.__parse_list(data[0]) # [attribs, hierarchy delimiter, root name]
        hierarchy_delim = lst[1]
        # NIL if there is no hierarchy
        if 'NIL' == hierarchy_delim:
            hierarchy_delim = '.'

        return hierarchy_delim

    def __convert_utf7(self, str):
        p=str.split("&")
        rst=p[0]+("+"+p[1]).decode("utf-7")
        return rst

    def get_mailbox_names(self):
        delim = self.__get_hierarchy_delimiter()

        # Get LIST of all folders
        typ, data = self.__server.list()
        assert(typ == 'OK')

        names=[]
        # parse each LIST, find folder name
        for row in data:
            lst = self.__parse_list(row)
            foldername = lst[2]

            unicodename=foldername
            if foldername.find('&')>=0:
                unicodename=self.__convert_utf7(foldername)

            names.append((foldername,unicodename))
        return names

    def get_mail(self, mailbox, dir, fromid=1):
        type, num= self.__server.select(mailbox)
        #type, data =self.__server.search(None,"ALL")

        for mail_id in range(fromid,int(num[0])+1):
            type=None
            try:
                type, msg_data=self.__server.fetch(mail_id, "(RFC822 BODY[TEXT])")
            except:
                print "Get ", mail_id, " failed"

            if type!="OK":
                continue

            content=msg_data[0][1]
            bodyMD5=msg_data[1][1].strip()

            print "Saving ",mail_id, " total ", num[0]

            filename=hashlib.md5(bodyMD5).hexdigest()
            file=open(os.path.join(dir,filename), "wb")
            file.write(content)
            file.flush()
            file.close()

    def upload_mail(self, mailbox, content):
        print self.__server.append(mailbox,0,0,content)
```

先是下载邮件：

``` python
def getAllMails(receive_folder,sent_folder):
    src_svr=IMAPServer("imap.gmail.com",993, True, "gmail@gmail.com", "gmail")
    for mailbox, unicode in src_svr.get_mailbox_names():
        print mailbox, "t" , unicode

    src_svr.get_mail('INBOX', receive_folder)
    src_svr.get_mail('[Gmail]/&XfJT0ZCuTvY-', sent_folder)

    src_svr=IMAPServer("imap.163.com",993, True, "163", "163")
    for mailbox, unicode in src_svr.get_mailbox_names():
        print mailbox, "t" , unicode

    src_svr.get_mail('INBOX', receive_folder)
    src_svr.get_mail('Sent Items', sent_folder)

    print "Done"
```

待所有的邮件下载完成以后，再上传：

```
    def copyto(receive_folder, sent_folder):
        to_receive_folder="D:\mail\to_receive"
        to_sent_folder="D:\mail\to_sent"

        to_svr=IMAPServer("imap.qq.com", 993, True, "QQ号码", '密码')

        print "Upload"
        finished=0
        total=len(os.listdir(receive_folder))
        for received in os.listdir(receive_folder):
            finished+=1
            print finished, "t", total

            path=os.path.join(receive_folder, received)
            file=open(path, "r")
            content=file.read()
            file.close()
            to_svr.upload_mail('INBOX',content)
            os.remove(path)

        finished=0
        total=len(os.listdir(sent_folder))
        for sent in os.listdir(sent_folder):
            finished+=1
            print finished, "t", total

            path=os.path.join(sent_folder, sent)
            file=open(path, "r")
            content=file.read()
            file.close()
            to_svr.upload_mail('&XfJT0ZABkK5O9g-',content)
            os.remove(path)
```

主程序如下：

```
    def main():
        """Main entry point"""

        receive_folder="D:\mail\received\"
        sent_folder="D:\mail\sent"

        getAllMails(receive_folder,sent_folder)  #先把所有的邮件下载完成后，再用copyto函数
        #copyto(receive_folder,sent_folder)

    if __name__ == '__main__':
      main()
```

这样就可以挪移咯。经过实验，将我的所有的2300多个邮件全部转移到QQ邮箱，并且发送和接受都分开存储。
当然这个脚本还有很多不足，比如MD5的方式比较内容还是有点问题。例如你把GMAIL的邮件下载下来放到QQ上去再下载下来，你就会发现BODY最后的部分多了几个空格，MD5的方式就不能检测出这种相同的邮件。
控制台的方式显然比较麻烦，最好的办法还是做一个GUI来进行同步处理。只是我没有那么多时间来整这个了，有兴趣的朋友可以发挥你们的才能。
