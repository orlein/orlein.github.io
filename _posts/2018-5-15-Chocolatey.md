---
layout: post
title: Chocolatey
categories: [Tech, Package]
---

# Chocolatey
----------------------

# 1. What is Chocolatey? Chocolatey란?

> [Chocolatey](https://chocolatey.org/about) is package manager for Windows.

> [Chocolatey](https://chocolatey.org/about) 는 윈도우즈에서 쓰는 패키지관리자입니다.

# 2. What is Chocolatey for? Chocolatey를 어디다 쓰나요?

> To install and update software, and delete installed software.

> 소프트웨어를 설치하거나 업데이트, 삭제할 수 있습니다.

# 3. How to install chocolatey 설치방법

(Windows 7)
1. open cmd.exe as administrator 관리자로 cmd 실행

2. Copy and paste below, and press enter. 아래의 코드를 복사-붙여넣기하고 엔터만 누르면 됨.
    <pre><code> @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
</code></pre>

3. Done!

Or.... 또는...

(Windows 10)

1. open powershell as administrator 관리자로 powershell 실행

2. Copy and paste below, and press enter. 아래의 코드를 복사-붙여넣기하고 엔터만 누르면 됨.
    <pre><code>Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
</code></pre>

3. Done!

#4. How to install package 패키지 설치방법

1. open cmd or powershell as administrator 관리자로 cmd나 powershell 실행

2. Find some packages in https://chocolatey.org/packages 링크에서 패키지 찾기

3. type below... 아래의 코드 실행

    <pre><code>
    choco install -y &lt;packagename1&gt; &lt;packagename2&gt; &lt;packagename3&gt; ...
    </code></pre>

  * example예시: 

    <pre><code>
    choco install -y jre8 googlechorme 7zip.install git.install nodejs.install openssh
    </code></pre>

4. Done! 끝남ㅋ



It's super easy. 엄청나게 간편함 ㅋ