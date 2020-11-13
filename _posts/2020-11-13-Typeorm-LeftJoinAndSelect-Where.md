---
layout: post
title: Typeorm LeftJoinAndSelect Where
categories: [Tech, Typeorm, SQL]
---


Typeorm LeftJoinAndSelect Where
-------------------

Assume you have entities like this:

우리가 이런 Entity를 선언했다 치자.

```TypeScript
@Entity()
export class User extends BaseEntity {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @OneToMany((_) => Vote, (vote) => vote.voter)
  votes: Vote[];
}

@Entity()
export class Post extends BaseEntity {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @OneToMany((_) => Vote, (vote) => vote.post)
  votes: Vote[];
}

@Entity()
export class Vote extends BaseEntity {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @ManyToOne((_) => Post, (post) => post.votes)
  post: Post;

  @ManyToOne((_) => User, (user) => user.votes)
  voter: User;
}
```

User votes to the Post, and each Vote has one User as voter.

User는 Post에 Vote를 할 수 있는데, 각 Vote는 Voter가 있다.

Of course, there is "writer" relation between User and Post, but we have to hide it now.

물론 "writer" 관계가 User와 Post사이에 있지만 지금은 숨기도록 한다.

When you have to get the result whether the specific User votes the Post(s) or not, what should you do?

만약 특정 User가 Post 혹은 Post들에 Vote를 했는지 안했는지 알려면 어떻게 해야하는가?

```TypeScript
const result = await getRepository(Post)
  .createQueryBuilder("post")
  .leftJoinAndSelect("post.upvotes", "upvote", "upvote.upvoter.userId = :specificUserId", { specificUserId: "some id" })
  .leftJoinAndSelect("upvote", "upvoter")
  .getMany()
```

You can just refer deep property as if you can get the result by JavaScript Object.

마치 자바스크립트 객체로 결과를 받은것처럼 그냥 깊은 property를 참조해서 비교해주면 된다.
