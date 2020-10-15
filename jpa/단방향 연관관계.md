# 단방향 연관관계
엔티티의 관계가 한쪽만 참조하는 것을 단방향 관계라고 한다.  

## 객체의 연관관계와 테이블에서의 연관관계의 차이

<img src="https://github.com/yangseungin/TIL/blob/master/jpa/%EC%82%AC%EC%A7%84/%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84/%EB%8B%A4%EB%8C%80%EC%9D%BC%20%EB%8B%A8%EB%B0%A9%ED%96%A5.png?raw=true" width="80%"> 

객체는 참조를 통해 연관관계를 맺어 언제나 단방향이다. 따라서 객체 간 연관관계를 양방향으로 만들려면 반대쪽도 필드를 추가해야 한다.  
하지만 테이블에서는 외래키를 통해 연관관계를 맺으며 A join B가 되면 B join A도 가능하다.
## 객체 관계 매핑
~~~ java
@Entity
public class Member {
  @Id @GeneratedValue
  private Long id;
  
  @Column(name = "USERNAME")
  private String name;
    
  @ManyToOne
  @JoinColumn(name = "TEAM_ID")
  private Team team;
  ...
}
@Entity
public class Team {
  @Id @Column(name = "TEAM_ID")
  private String id;
  
  private String name;
}
~~~
@ManyToOne: 다대일 관계를 의미하며 Member입장에서 Team은 다대일이다.  
@JoinColumn(name="TEAM_ID"): 외래키를 매핑할때 사용하며 Team 테이블의 TEAM_ID이라는 이름의 외래키와 매핑한다.  
아래 사진은 객체의 참조와 테이블의 외래키를 연관관계 매핑한 후의 모습이다.  

<img src="https://github.com/yangseungin/TIL/blob/master/jpa/%EC%82%AC%EC%A7%84/%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84/%EA%B0%9D%EC%B2%B4%20%EA%B4%80%EA%B3%84%20%EB%A7%A4%ED%95%91.png?raw=true" width="80%">  

# 연관관계 사용
## 연관관계 저장
~~~ java
Team team = new Team();
team.setName("TeamA");
em.persist(team);   //팀 저장
​
Member member = new Memeber();
member.setName("member1");
member.setTeam(team);//단방향 연관관계 설정
em.persist(member);

//1차캐시 정리
em.flush(); 
em.clear()

Member findMember = em.find(Member.class,  member.getId());
//참조를 사용해서 연관관계 조회
Team findTeam = findMember.getTeam();
~~~
Team과 Member를 저장 후 연관관계를 설정하였다. 정상적으로 저장됐는지 확인하기 위해 select 쿼리를 확인하기 위해 1차 캐시를 Clear후 find를 하여 회원과 팀이 정상적으로 저장되었음을 확인할 수 있다.

## 연관관계 수정
~~~ java
// Member1은 TeamA에 소속된 상태

//새로운 팀 등록
Team newTeam = new Team();
team.setName("TeamB");
em.persist(team);  

//Member1의 팀을 TEAMB로 변경
Member member = em.find(Member.class, "member1");
member.setTeam(newTeam);
~~~
새로운 팀을 저장한 후 member1의 팀을 2로 설정만 하였다.  
다시 저장하거나 update하는 메서드 없이 엔티티의 값을 변경하면 트랜잭션을 커밋 할때 flush가 발생하면서 변경을 감지하고 DB에 자동으로 업데이트된다.

## 연관관계 제거 및 엔티티 삭제
~~~ java
Member member1 = em.find(Member.class, "member1");
member1.setTeam(null)// 연관관계 제거

Team teamA = em.find(Team.class, "teamA");
em.remove(teamA) //연관관계 삭제
~~~
연관관계를 null로 설정하면 TEAM_ID를 null로 하는 update쿼리가 발생한다.  
엔티티를 삭제할 때는 외래키 제약조건으로 인해 연관관계를 모두 제거한 후 삭제해야 한다.



# 참고문서
자바 ORM 표준 JPA 프로그래밍 - 김영한  
https://www.inflearn.com/course/ORM-JPA-Basic/