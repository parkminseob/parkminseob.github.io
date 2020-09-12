---

title: 자바 미니 프로젝트 - 성적 조회 프로그램
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
article_tag2: project
tags:
  - Java
  - project
---

# 성적조회 프로그램

24시간 미니 해커톤을 진행하며 간단한 성적조회 프로그램을 만들었다.

간단하지만 아직 프로그래밍 레벨1인 나에겐 어려웠던 CRUD구현.. 그리고 내가 구상했던 3가지 새로운 기능(1등출력, 꼴찌출력, 선생님 코멘트) 중 1가지는 구현하지 못했지만 나름대로 최선을 다했다. 몸이 아프지 않았다면 좀 더 집중력있게 만들 수 있을텐데 하는 아쉬움도 남지만.. 앞으로도 내게 남은 기회가 많으니까! 적당히 만족하기로 했다.



## App클래스

프로그램을 시작하면 가장 먼저 보이는 화면. 1번을 누르면 선생님 로그인 창으로 이동한다.

```java
public class App {
  public static void main(String[] args) {


    Member member = new Member();

    loop:
      while(true) {
        System.out.println("[고등학교 성적관리시스템 메인]");
        System.out.printf("[1] 선생님 로그인    [0] 종료\n");
        int command = Prompt.inputInt("명령> ");

        switch(command) {
          case 1: member.System(); break;

          case 0 : 
            System.out.println("프로그램 종료");
            break loop;
          default : System.out.println("잘못된 명령입니다."); 
        }
        System.out.println();
      }
  }
}

```

## TeacherLogin 클래스

필드는 private로 막고 게터세터를 썼다. add()메서드로 회원가입을 할 수 있게 구현했다.

```java
package mini.project;

public class TeacherLogin {
	private String Id;
	private String Password;
	private String Name;

	public TeacherLogin(String Id, String Password, String Name) {
		super();
		this.Id = Id;
		this.Password = Password;
		this.Name = Name;
	}
	public String getId() {
		return Id;
	}
	public void setId(String Id) {
		this.Id = Id;
	}
	public String getPassword() {
		return Password;
	}
	public void setPassword(String Password) {
		this.Password = Password;
	}
	public String getName() {
		return Name;
	}
	public void setName(String Name) {
		this.Name = Name;
	}
	@Override
	public String toString() {
		return "ID : " + Id + " Password : " + Password + " Name : " + Name;
	}
	public void add() {
		TeacherLogin t = new TeacherLogin(Id, Password, Name);
		t.Id = Prompt.inputString("Id : ");
		t.Password = Prompt.inputString("Password : ");
		t.Name = Prompt.inputString("Name : ");
	}
}

```

## Teacher 클래스

기본 관리자를 생성자로 만들어놓았다. 그리고 회원가입을 통해 로그인하여 학생정보 관리시스템에 접속할 수 있도록 구현했다.

```java
package mini.project;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Scanner;


public class Teacher {

	List<TeacherLogin> teachers = new ArrayList<>();
	Scanner scanner = new Scanner(System.in);

	public Teacher() {
		teachers.add(new TeacherLogin("admin", "1234", "Kim"));
	}

	public void System() {
		int num = 0;
		while ((num = menu()) != 0) {
			switch (num) {
			case 1:
				TeacherLogin();
				break;
			case 2:
				TeacherJoin();
				break;
			case 3:
				TeacherList();
			}
		}
	}

	private void TeacherJoin() {
		String Id = Prompt.inputString("ID : ");
		String Password = Prompt.inputString("Password : ");
		String Password2 = Prompt.inputString("Password Confirm : ");
		String Name = Prompt.inputString("Name : ");

		if(TestId(Id)) {
			System.out.println("중복된 ID입니다.");
		} else if(Password.equals(Password2)) {
			teachers.add(new TeacherLogin(Id, Password, Name));
			System.out.println("회원가입이 완료되었습니다.");
		} else {
			System.out.println("비밀번호를 다시 입력하시오.");
		}
	}

	private void TeacherLogin() {
		while (true) {
			String Id = Prompt.inputString("ID : ");

			String Password = Prompt.inputString("Password : ");

			TeacherLogin member = FindId(Id);
			StudentHandler stu = new StudentHandler();

			if (member == null) {
				System.out.println("입력하신 아이디가 일치하지 않습니다.");
				System.out.println("다시 입력하시오.");
			} else if(member.getPassword().equals(Password)) {
				System.out.println("학생정보관리시스템에 접속하였습니다.");
				stu.StudentSystem();
				break;
			} else {
				System.out.println("입력하신 패스워드가 일치하지 않습니다.");
				System.out.println("다시 입력하시오.");
			}
		}
	}

	public boolean TestId(String Id) {
		boolean check = true;
		TeacherLogin member = FindId(Id);
		if (member == null)
			check = false;
		return check;

	}

	private TeacherLogin FindId(String Id) {
		for(TeacherLogin memberLogin : teachers) {
			if(memberLogin.getId().equals(Id)) {
				return memberLogin;
			}
		}
		return null;
	}

	public int menu() {
		return Prompt.inputInt("[1]로그인 [2]회원가입 [3]전체회원 [0]종료");
	}

	public void TeacherList() {
		System.out.println("[관리자 목록]");

		Iterator<TeacherLogin> ite = teachers.iterator();

		while(ite.hasNext()) {
			System.out.println(ite.next() + " ");
		}
	}
}

```

## Student클래스

필드를 private로 막고 게터세터를 사용한 것은 위 Teacher클래스와 동일하다. 

다만 Student클래스에서 국영수 점수의 총합인 sum과 평균인 aver은 메서드를 용도에 맞게 수정해 주었다.

```java
package mini.project;

import java.sql.Date;

public class Student {

  private int no;
  private String name;
  private int gender;
  private String grade;
  private int kor;
  private int eng;
  private int math;
  private int sum;
  private float aver;
  private String comment;

  private Date registeredDate;

  private static final int LENGTH = 5;

  public String getComment() {
    return comment;
  }

  public void setComment(String comment) {
    this.comment = comment;
  }

  public int getSum() {
    return sum;
  }

  public void setSum(int kor, int eng, int math) {
    sum = kor+eng+math;
    aver = sum/3.f;
  }

  public void setAver(float aver) {
    this.aver = aver;
  }

  public float getAver() {
    return aver;
  }

  public int getNo() {
    return no;
  }

  public void setNo(int no) {
    this.no = no;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getGender() {
    return gender;
  }

  public void setGender(int gender) {
    this.gender = gender;
  }

  public String getGrade() {
    return grade;
  }

  public void setGrade(String grade) {
    this.grade = grade;
  }

  public int getKor() {
    return kor;
  }

  public void setKor(int kor) {
    this.kor = kor;
  }

  public int getEng() {
    return eng;
  }

  public void setEng(int eng) {
    this.eng = eng;
  }

  public int getMath() {
    return math;
  }

  public void setMath(int math) {
    this.math = math;
  }

  public Date getRegisteredDate() {
    return registeredDate;
  }

  public void setRegisteredDate(Date registeredDate) {
    this.registeredDate = registeredDate;
  }

  public static int getLength() {
    return LENGTH;
  }
}

```

## StudentHandler 클래스

학생 정보와 성적을 입력받고 목록조회, 상세조회, 정보수정, 정보삭제와 같은 기본적인 CRUD를 구현했다.
그리고 이 프로그램만의 특징인 1등과 꼴찌, 선생님 코멘트 기능을 추가했다. 

* 1등 출력은 학년 상관 없이 국영수 총합으로만 따져서 출력하도록 했다.
* 꼴찌 출력은 기능이 제대로 구현이 되지 않았다.
  추측을 해보자면 정보를 추가하지 않은 인덱스의 값이 다 0으로 초기화 되었기에 무조건 최소값이 0으로 출력되는 듯 하다. 아직은 해결 방법을 찾지 못했다.
* 선생님 코멘트는 학생 번호를 통해 입력하고, 학생 상세 조회에서 볼 수 있도록 구현했다. 

```java
package mini.project;

import java.sql.Date;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class StudentHandler {
  List<Student> studentList = new ArrayList<Student>();


  public void StudentSystem() {
    int num = 0;
    loop:
      while((num = StudentMenu()) != 0) {
        switch (num) {
          case 1: add(); break;
          case 2: scoreAdd(); break;
          case 3: studentList(); break;
          case 4: StudentDetail(); break;
          case 5: studentUpdate(); break;
          case 6: studentDelete(); break;
          case 7: FirstPlace(); break;
          case 8: BottomPlace(); break;
          case 9: teacherComment(); break;

          case 0: System.out.println("학생등록시스템 종료"); break loop;
          default : System.out.println("잘못된 명령입니다."); 
        }
        System.out.println();
      }
  }

  public int StudentMenu() {
    System.out.println("**0번을 누르면 메인 창으로 이동됩니다.**");
    return Prompt.inputInt(
        "[1]학생 정보 입력"
            + " [2]학생 성적 입력"
            + " [3]학생 목록"
            + " [4]학생 상세 조회\n"
            + "[5]학생 정보 수정"
            + " [6]학생 정보 삭제"
            + " [7]1등은? "
            + " [8]꼴찌는? "
            + " [9]선생님 코멘트");
  }

  private void scoreAdd() {
    System.out.println("[학생 성적 입력]");
    int no = Prompt.inputInt("학생 번호? ");
    Student student = findByNo(no);

    if(student == null) {
      System.out.println("해당 번호의 학생이 없습니다.");
      return;
    }

    student.setKor(Prompt.inputInt("국어성적? "));
    student.setEng(Prompt.inputInt("영어성적? "));
    student.setMath(Prompt.inputInt("수학성적? "));
    student.setSum(student.getKor(), student.getEng(), student.getMath());
  }

  private void add() {
    System.out.println("[학생 정보 입력]");

    Student student = new Student();

    while(true) {
      int number = Prompt.inputInt("학생 번호?(0 : 등록취소) ");
      if (number == 0) {
        System.out.println("학생 등록을 취소합니다.");
        return;
      } else if(this.indexOf(number) != 0) {
        student.setNo(number);
        break;
      } else {
        System.out.println("이미 등록된 학생번호입니다.");
      }
    }

    student.setName(Prompt.inputString("학생 이름? "));
    student.setGender(Prompt.inputInt("학생 성별? (1: 남자 / 2 : 여자)"));

    String grade = Prompt.inputString("학생 학년? ");
    int num = Integer.parseInt(grade);
    if(num < 0 || num > 4) {
      System.out.println("잘못된 입력값입니다.");
      return;
    } else {
      student.setGrade(grade);
    }

    student.setRegisteredDate(new Date(System.currentTimeMillis()));

    studentList.add(student);
  }

  public void studentList() {
    System.out.println("[학생 목록]");
    String grade = Prompt.inputString("**학년을 입력해주세요.**");
    Iterator<Student> iterator = studentList.iterator();


    while(iterator.hasNext()) {
      Student student = iterator.next();
      if(student.getGrade().equals(grade)) {

        String genderLabel = null;
        switch(student.getGender()) {
          case 1: genderLabel = "남자"; break;
          case 2: genderLabel = "여자"; break;
        }

        System.out.printf("이름 : %s\n성별 : %s\n학년 : %s\n국어 : %d\n영어 : %d\n"
            + "수학 : %d\n합계 : %d\n평균 : %.1f\n등록일 : %s\n\n",
            student.getName(),
            genderLabel,
            student.getGrade(),
            student.getKor(),
            student.getEng(),
            student.getMath(),
            student.getSum(),
            student.getAver(),
            student.getRegisteredDate());
      }
    }
  }

  public void StudentDetail() {
    System.out.println("[학생 상세 조회]");
    int no = Prompt.inputInt("학생 번호? ");
    Student student = findByNo(no);

    if(student == null) {
      System.out.println("해당 번호의 학생이 없습니다.");
      return;
    }

    System.out.printf("번호 : %s\n", student.getNo());
    System.out.printf("이름 : %s\n", student.getName());
    String genderLabel = null;
    switch(student.getGender()) {
      case 1: genderLabel = "남자"; break;
      case 2: genderLabel = "여자"; break;
    }
    System.out.printf("성별 : %s\n", genderLabel);
    System.out.printf("학년 : %s\n", student.getGrade());
    System.out.printf("국어 : %s\n", student.getKor());
    System.out.printf("영어 : %s\n", student.getEng());
    System.out.printf("수학 : %s\n", student.getMath());
    System.out.printf("총합 : %s\n", student.getSum());
    System.out.printf("평균 : %s\n", student.getAver());

    if(student.getComment() != null) {
      System.out.printf("코멘트 : %s\n", student.getComment());
    }
  }

  public void studentUpdate() {
    System.out.println("[학생 정보 변경]");
    int no = Prompt.inputInt("학생 번호? ");
    Student student = findByNo(no);

    if(student == null) {
      System.out.println("해당 번호의 학생이 없습니다.");
      return;
    }
    String name = Prompt.inputString(
        String.format("이름(%s)? ", student.getName()));
    String grade = Prompt.inputString(
        String.format("학년(%s)? ", student.getGrade()));
    int kor = Prompt.inputInt(
        String.format("국어(%s)? ", student.getKor()));
    int eng = Prompt.inputInt(
        String.format("영어(%s)? ", student.getEng()));
    int math = Prompt.inputInt(
        String.format("수학(%s)? ", student.getMath()));

    student.setSum(kor, eng, math);

    String response = Prompt.inputString("정말 변경하시겠습니까?(y/N) ");
    if (!response.equalsIgnoreCase("y")) {
      System.out.println("변경을 취소하였습니다.");
      return;
    }
    student.setName(name);
    student.setGrade(grade);
    student.setKor(kor);
    student.setEng(eng);
    student.setMath(math);
    student.getSum();

    System.out.println("학생 정보를 변경하였습니다.");
    System.out.println();
  }

  public void studentDelete() {
    System.out.println("[학생 정보 삭제]");
    int no = Prompt.inputInt("번호? ");
    int index = indexOf(no);

    if(index == -1) {
      System.out.println("해당 번호의 학생이 없습니다.");
      return;
    }

    String response = Prompt.inputString("정말 삭제하시겠습니까?(y/N) ");
    if(!response.equalsIgnoreCase("y")) {
      System.out.println("삭제 취소.");
      return;
    }

    studentList.remove(index);
    System.out.println("삭제 완료.");
    System.out.println();
  }

  public void FirstPlace() {
    System.out.println("[1등 출력]");
    Iterator<Student> iterator = studentList.iterator();

    int[] first = new int[studentList.size()];
    int max = first[0];
    String name = null;
    while(iterator.hasNext()) {
      Student student = iterator.next();
      if(student.getSum() > max) {
        max = student.getSum();
        name = student.getName();
      }
    }

    System.out.printf("1등은? ==> %s : %d점\n",name, max);
    System.out.println("와우! 대단해요!");
    System.out.println();
  }

// 제대로 구현되지 못한 메서드
  public void BottomPlace() {
    System.out.println("[꼴지 출력]");

    int[] bottom = new int[studentList.size()];
    int min = bottom[0];
    String name = null;
    for(int i = 1; i < studentList.size(); i++) {
      Student student = studentList.get(i);

      if(student.getSum() < min) {
        min = student.getSum();
        name = student.getName();
      }
    }
    System.out.printf("꼴지 ==> %s : %d\n", name, min);
    System.out.println("이 학생은 격려가 필요합니다.");
    System.out.println();
  }

  private void teacherComment() {
    System.out.println("[선생님 코멘트]");
    int no = Prompt.inputInt("코멘트 입력할 학생 번호? ");
    Student student = findByNo(no);

    if(student == null) {
      System.out.println("해당 번호의 학생이 없습니다.");
      return;
    }
    student.setComment(Prompt.inputString("한줄 코멘트를 입력하세요.\n"));
  }

  private Student findByNo(int no) {
    for(int i = 0; i < studentList.size(); i++) {
      Student student = studentList.get(i);
      if(student.getNo() == no) {
        return student;
      }
    }
    return null;
  }

  private int indexOf(int no) {
    for (int i = 0; i < studentList.size(); i++) {
      Student student = studentList.get(i);
      if (student.getNo() == no) {
        return i;
      }
    }
    return -1;
  }
}

```

이렇게 24시간, 정확히는 잠자고 밥먹는 시간을 제외한 14시간만에 팀원 한명과 태그를 이뤄 성적조회 프로그램을 만들었다. 

미니 해커톤이지만 긴장감을 가지고 처음으로 누군가와 협업을 하며 프로그램을 어떻게 구현할지 고민하는 시간을 가질 수 있었다.


특히 간단하게만 생각했던 CRUD조차도 막상 처음부터 만들자니 자잘한 버그가 많이 발생했고, 그런 버그들을 고치는데 시간이 많이 들었다. 기능을 만드는 것 보다 유효성 검사를 꼼꼼히 해주며 프로그램이 단단해지도록(?) 하는게 더 어려웠다.

미숙하지만 결과물을 내고 나니 한층 더 자신감이 붙는걸 느낄 수 있었다. 재밌었음!

