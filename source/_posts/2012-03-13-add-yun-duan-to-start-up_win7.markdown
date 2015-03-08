---
author: hesicong
comments: true
date: 2012-03-13 09:12:01+00:00
layout: post
title: 添加云端到开始菜单(WIN7)
wordpress_id: 3044
categories:
- 生活
---

```
import re
import os
import configparser
import pythoncom
import sys
from win32com.shell import shell
from win32com.shell import shellcon

import win32com.client
objShell = win32com.client.Dispatch("WScript.Shell")
allUserProgramsMenu = objShell.SpecialFolders("AllUsersPrograms")

if len(sys.argv) < 2:
print("请输入CloudCache路径")
sys.exit(1)

def buildShortCut(lnkFile, name, appdir, para, workdir, showcmd):
print(lnkFile,name,appdir,para,workdir,showcmd)
shortcut = pythoncom.CoCreateInstance(shell.CLSID_ShellLink, None, pythoncom.CLSCTX_INPROC_SERVER, shell.IID_IShellLink)
shortcut.SetPath(appdir)
shortcut.SetDescription(name)
shortcut.SetWorkingDirectory(workdir)
shortcut.SetArguments(para)
shortcut.QueryInterface(pythoncom.IID_IPersistFile).Save(lnkFile,0)

find_file=re.compile(r"init.ini")
find_path=sys.argv[1]
find_walk=os.walk(find_path)

shortCutDir = allUserProgramsMenu

iniFiles = []
for path,dirs,files in find_walk:
for file in files:
if find_file.search(file):
iniFilePath = os.path.join(path, file)
print(iniFilePath)
iniFileObj = open(iniFilePath)
iniContent = "rn".join(iniFileObj.readlines()[1:-1])
config = configparser.ConfigParser()
config.read_string(iniContent)
print(config.sections())

shortCutSubdir = ""
for sectionName in config.sections():
if sectionName == "Shortcut0":
shortCutSubdir = os.path.join(shortCutDir, config[sectionName]["ShortcutName"])

if sectionName.find("Shortcut") == 0:
section = config[sectionName]
packageName = config["Common"]["PacketName"]

if not os.path.exists(shortCutSubdir):
os.mkdir(shortCutSubdir)

buildShortCut(os.path.join(shortCutSubdir, section["ShortcutName"]+ ".lnk"), section["ShortCutName"],
section["AppDir"], section["Param"], section["WorkingDirectory"], 1)
```