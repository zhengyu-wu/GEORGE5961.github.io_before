---
layout: post
title: "Bookstore (1) Build a backend service using Springboot"
author: "Zhengyu Wu"
---

I built a bookstore website like Amazon last year. The backend is supported by Struts, Spring, and Hibernate (SSH). Recently, I am reconstructing the website using **Springboot**. Ideally, an enterprise-level information system. A series of articles will show the information system in detail. You can find the code [here](https://github.com/GEORGE5961/Bookstore).

## What is Struts, Spring, and Hibernate (SSH)?

* Struts: a framework of Web MVC (model, view, controller) 
* Spring: manage the relationships between Java classes using DI (dependency injection) like a glue 
	* You are freed of considering memory allocation, life cycle of a object, and garbage collection.
	* How to do: register a class as a Java bean, then spring will manage the bean. You just need to declare a object like the below then the object will be injected in democlass

```
public class democlass{
	private TestClass tc;
}
```
	

* Hibernate: map a class and a table in a database
	* You never ever bother writing a SQL sentence.

## Springboot

SSH is an out-dated stuff which has been replaced by Springboot.

Springboot is amaming in that:

* You do not configure those xml files any more.
* You can use **annotations** to do a lot of things.
* You write less codes.

## Backend demo

Our current goal:

* Map a RESTFUL url to an action in the backend
* Connect the backend and MYSQL database
* CRUD (create, read, update, delete) operations 

### Step 1: Create a springboot project

It seems simple in this step. Actually, this step may be very confusing. Many tutorials mislead readers and I’m a victim as well. Therefore, I decide to go into the details.

**First**, click on the button "Create New Project". Note that clicking "Import Project" and "Open" differs in that "Import Project" will configure the project in some unexpected way while "Open" opens files like an editor. It is recommended to use "Open" then configure the dependencies. This is what painful experiences tell me.

<img src="https://github.com/GEORGE5961/markdown_photos/blob/master/spr1.png?raw=true" width="60%"/>


**Second**, choose "Spring Initializr" like in the image. I got really despaired when first time saw this page.

<img src="https://github.com/GEORGE5961/markdown_photos/blob/master/spr2.png?raw=true" width="60%"/>

**Third**, notice the "Package", this will be the name of your main directory.

<img src="https://github.com/GEORGE5961/markdown_photos/blob/master/spr3.png?raw=true" width="60%"/>

**Fourth**, choose "web". If you wanna use other applications you can add the maven dependenies later.

<img src="https://github.com/GEORGE5961/markdown_photos/blob/master/spr4.png?raw=true" width="60%"/>

### Step 2: Organize the dictionary


Organize the dictionary like below:

<img src="https://github.com/GEORGE5961/markdown_photos/blob/master/spr5.png?raw=true" width="40%"/>

It demonstrates the idea of **layering**.

* Controller: map a RESTFUL URL to a service
* Model: define objetcs 
* Repository: CRUD operations to synchronize objects with databases 
* Service: define business logic services by combining operations in Repository layer

### Step 3: Coding

Before coding, you should know the meanings of anotations.

Springboot annotations:

1. @SpringBootApplication
	* A necessary configuration for springboot
	* @SpringBootApplication = @Configuration + @EnableAutoConfiguration + @ComponentScan 
2. @Configuration：equals to the traditional xml files
3. @AutoWired：auto inject a registered bean
4. @RestController + @RequestMapping(“/test")
    * Always used together
    * @RestController: methods in a class return a json 
5. @Component @Controller @Repository @Service
    * Define beans的 (without these four annotations，a class cannot be registered as a bean and cannot be injected to other classes using @autowired)
    * @Component: most general
    * @Service, @Repository: is a type of @Component but with different meanings 
    	* For reading code conveniently
    	* In the aspect of compiling, @Component, @Service, @Repository will not cause errors even if using @Service in Repository layer
    	* @Service used in Service layer, @Repository used in Repository layer
    * @Controller: cannot be replaced by @Component, @Repository, or @Service
    	* @RestController includes @Controller


You also need to get familiar with JPA. JPA is for operating data in databases. Note that I just declare a function like in UserRepository.java and don't implement it after extending JPA.




Take User services as an instance.

In UserController.java:

```
package com.zhengyu.bookstore.Controller;

import com.zhengyu.bookstore.model.User;
import com.zhengyu.bookstore.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

	@Autowired
	private UserService userService;

	@ResponseBody
	@PostMapping("/addUser")
	public User addUser(User user)
	{
		return userService.insert(user);
	}

	@ResponseBody
	@GetMapping("/getAllUsers")
	public List<User> getAllUser()
	{
		return userService.getAllUsers();
	}

	@ResponseBody
	@PostMapping("/deleteByUserId")
	public boolean deleteByUserId(int userId){
		return userService.deleteUser(userId);
	}

	@ResponseBody
	@PostMapping("/updateUser")
	public void updateUser(User user)
	{
		userService.updateUser(user);
	}

}
```

In User.java:

```
package com.zhengyu.bookstore.model;

import javax.persistence.*;
import java.util.Objects;


@Entity
@Table(name = "users")
public class User {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;

	@Column(nullable = false)
	private String username;

	@Column(nullable = false)
	private String password;

	@Column(nullable = false)
	private String role;

	@Column(nullable = false)
	private String intro;

	public User() {
	}

	public User(String username, String password, String role, String intro) {
		this.username = username;
		this.password = password;
		this.role = role;
		this.intro = intro;
	}


	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getRole() {
		return role;
	}

	public void setRole(String role) {
		this.role = role;
	}

	public String getIntro() {
		return intro;
	}
	
	public void setIntro(String intro) {
		this.intro = intro;
	}
	
}
```

In UserRepository.java:

```
package com.zhengyu.bookstore.repository;

import com.zhengyu.bookstore.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface UserRepository extends JpaRepository<User,Integer> {
	User findById(int userId);
}
```

In UserService interface:

```
package com.zhengyu.bookstore.service;

import com.zhengyu.bookstore.model.User;

import java.util.List;


public interface UserService {

    User insert(User user);

    boolean deleteUser(int id);

    void updateUser(User user);

    User getUserById(int id);

    List<User> getAllUsers();

}
```
In UserServiceImpl.java:

```
package com.zhengyu.bookstore.service.impl;

import com.zhengyu.bookstore.model.User;
import com.zhengyu.bookstore.repository.UserRepository;
import com.zhengyu.bookstore.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;


@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserRepository userRepository;

    public User insert(User user) {

        try {
            //插入前先看看这个id在数据库中是否存在
            //存在则不在插入
            User tmpUser=userRepository.findById(user.getId());
            if(tmpUser!=null)
                return null;
            return userRepository.saveAndFlush(user);
        }
        catch (Exception e){
            return null;
        }
    }

    public boolean deleteUser(int id) {

        try {
            userRepository.deleteById(id);
            return true;
        }
        catch (Exception e){
            return  false;
        }
    }

    public void updateUser(User user) {
        userRepository.updateById(user.getUsername(),user.getId());
    }

    public User getUserById(int id) {
        return userRepository.findById(id);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
```

Finally, I highly recommend you to read the references.

### Reference

1.[https://www.zhihu.com/question/21142149](https://www.zhihu.com/question/21142149)
2.[https://www.cnblogs.com/ityouknow/p/5662753.html](https://www.cnblogs.com/ityouknow/p/5662753.html)















