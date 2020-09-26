---

title: 자바 - File클래스
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



# File 클래스

* 파일이나 디렉토리 정보를 관리한다.(생성,삭제,변경 등..)

- 현재 폴더 경로는 `.` 으로, 상위 폴더 경로는 `..` 로 표시한다.
- `mkdir()` : 디렉토리를 생성한다. 상위 디렉토리가 존재하지 않는다면 하위 디렉토리는 생성할 수 없다. `mkdirs()`를 이용하면 존재하지 않는 중간 디렉토리도 생성할 수 있다.
- `delete()` : 디렉토리 안에 파일이나 다른 하위 디렉토리가 있다면 삭제할 수 없다.

* 파일을 생성하기 전에 존재하지 않는 폴더를 생서하고 싶다면 수퍼디렉토리 경로를 적어준다.

  ```java
      File file = new File("temp/b/test.txt");
      File dir = file.getParentFile();
  	dir.mkdirs()
  	file.createNewFile()
  ```



## File Filter

특정 파일만 추출하고 싶을때 사용하는 인터페이스이다. 

이 인터페이스에 정의된 accept()메서드는 listFiles()메서드에서 호출한다.

* 지정한 폴더에 들어 있는 파일이나 디렉토리를 찾을 때 마다 호출한다.
* 리턴 값 File[] 에 찾은 파일 정보를 포함시킬지 여부를 결정한다.
* true 이면 배열에 포함시키고 false 이면 배열에서 제외한다.
* FileNameFilter인터페이스도 있으나 이 필터는 이름으로만 검사하기 때문에 파일인지 디렉토리 여부는 검사하지 않는다. 그러므로 이름과 관계없이 원하는 확장자만 추출하고 싶다면 FileFilter를 쓰는 것이 더 낫다.



### File활용 : 재귀호출 - 트리구조

현재 디렉토리에서부터 하위 파일 및 디렉토리 목록을 알아낸다.

```java
// 활용 - 지정한 폴더 및 그 하위 폴더까지 모두 검색하여 파일 및 디렉토리 이름을 출력하라.
package com.eomcs.io.ex01;
import java.io.File;
public class Exam0710_07 {
  public static void main(String[] args) throws Exception {

    // 결과 예)
    // /Users/bitcamp/git/test
    // src/
    //   main/
    //     java/
    //       com/
    //         Hello.java
    //         Hello2.java
    // build.gradle
    // settings.gradle
    // Hello.java
    // ...
    
    File dir = new File(".");
    System.out.println(dir.getCanonicalPath());
    
    printList(dir, 1);
  }

  static void printList(File dir, int level) {
    // 현재 디렉토리의 하위 파일 및 디렉토리 목록을 알아낸다.
    File[] files = dir.listFiles();

    // 리턴 받은 파일 배열에서 이름을 꺼내 출력한다.
    for (File file : files) {
      printIndent(level);
      
      if (file.isDirectory() && !file.isHidden()) {
        System.out.printf("%s/\n", file.getName());
        printList(file, level + 1);
      } else {
        System.out.printf("%s\n", file.getName());
      }
    }
  }
  
  static void printIndent(int level) {
    for (int i = 0; i < level; i++) {
      System.out.print("  ");
    }
  }
}
```



### File 활용 - 폴더 삭제하기

File클래스는 상위폴더를 바로 삭제할 수 없다. 그러므로 주어진 파일이 디렉토리라면 하위 파일이나 디렉토리를 찾아 지우면서 순차적으로 상위 폴더로 올라와야 한다.

```java
package com.eomcs.io.ex01;
import java.io.File;
public class Exam0720 {
  public static void main(String[] args) throws Exception {

    File dir = new File("temp");

    deleteFile(dir);
  }

  static void deleteFile(File dir) {
    // 주어진 파일이 디렉토리라면 하위 파일이나 디렉토리를 찾아 지운다.
    if (dir.isDirectory()) {
      File[] files = dir.listFiles();
      for (File file : files) {
        deleteFile(file);
      }
    }

    dir.delete(); // 현재 파일이나 폴더 지우기
  }

}

```

