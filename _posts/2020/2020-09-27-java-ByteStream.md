---

title: 자바 - 입출력 스트림
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- Java
toc: true
toc_sticky: true
toc_label: on this page
article_tag1: Java
article_tag2: basic
tags:
  - Java
  - Java basic
---

![java-logo](https://user-images.githubusercontent.com/68311188/92201199-e4e6a200-eeb6-11ea-9f5b-76b79db3564f.png){:.aligncenter}

# 입출력 스트림(Stream)

프로그램이 출발지냐 또는 도착지냐에 따라서 사용하는 스트림의 종류가 결정된다.

* 프로그램이 도착지이면 흘러온 데이터를 입력받아야 하므로 **입력 스트림** 사용
* 프로그램이 출발지면 데이터를 출력해야 하므로 **출력 스트림** 사용



## 입출력 스트림의 종류

- 바이트(byte) 기반 스트림 : 그림, 멀티미디어 등의 바이너리 데이트를 읽고 출력할 때 사용
  - InputStream / OutputStream : 바이트 기반 입출력 스트림의 최상위 클래스
  - 이 클래스를 상속받은 서브클래스는 접미사로 InputStream / OutputStream 이 붙는다.
- 문자(character) 기반 스트림 : 문자 데이터를 읽고 출력할 때 사용
  - Reader / Writer : 문자 기반 입출력 스트림의 최상위 클래스
  - 이 클래스를 상속받은 서브클래스는 접미사로 Reader / Writer 이 붙는다.



> 바이너리 파일 vs 텍스트 파일
>
>  1) 바이너리 파일
>
> - 기본 텍스트 편집기(메모장, vi 에디터 등)로 편집할 수 없는 파일을 말한다.
> - 만약 텍스트 편집기로 변경한 후 저장하면, 파일 포맷이 깨지기 때문에 무효한 파일이 된다.
> - 예) .pdf, .ppt, .xls, .gif, .mp3, .jpg, .hwp, .mov, .avi, .exe, .lib 
> - 바이너리 파일을 편집하려면 해당 파일 포맷을 이해하는 전용 프로그램이 필요하다.
> 
>  2) 텍스트 파일
> - 기본 텍스트 편집기(메모장, vi 에디터 등)로 편집할 수 있는 파일을 말한다.
> - 예) .txt, .csv, .html, .js, .css, .xml, .bat, .c, .py, .php, .docx, .pptx, .xlsx 등
> - 텍스트 파일은 전용 에디터가 필요 없다.
> - 텍스트를 편집할 수 있는 에디터라면 편집 후 저장해도 유효하다.
>
> **바이너리 데이터 읽고, 쓰기**
> - 읽고 쓸 때 중간에서 변환하는 것 없이 바이트 단위로 그래도 읽고 써야 한다.
> - InputStream/OutputStream 계열의 클래스를 사용하라.
> 
> **텍스트 데이터 읽고, 쓰기**
> - 읽고 쓸 때 중간에서 문자 코드표에 따라 변환하는 것이 필요하다.
> - Reader/Writer 계열의 클래스를 사용하라.



### 바이트 스트림 (Byte Stream)

#### 1. InputStream

read() 메서드 : 입력 스트림으로부터 1byte를 읽고 int(4byte) 타입으로 리턴한다. 따라서 리턴된 4byte 중 **끝 1byte에만 데이터가 들어있다**.

![image](https://user-images.githubusercontent.com/68311188/94366650-9d99ad00-0114-11eb-9233-209611daa88b.png){:.aligncenter}