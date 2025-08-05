---
title: "Challenges I Faced During My Bulletin Board Project - 1"
date: 2025-08-05
categories: [Web]
tags: [Java, Spring Boot, Backend, Troubleshooting, JPA, Thymeleaf, HTML, JavaScript]
excerpt: "A brief summary of my experience developing a bulletin board web application using Java Spring Boot, including key technical challenges and how I overcame them."
---

You can see detailed information about the project [Here](https://github.com/Sunghak222/bulletin-board)

## Project Overview
This project was done for applying things I have recently studied, such as Java Spring Boot, Thymeleaf, HTML, Javascript, JPA, etc. The initial funcionalities that I planned to implement to the project are followed below:
- User registration / login / logout
- Create, read, update, delete posts
- Write, edit, delete comments
- Only authenticated users can write posts
- Only the author can edit or delete their content
- Input validation and exception handling
- RESTful API structure

I followed a traditional MVC pattern with Entity, Repository, Service, and Controller layers. There were no much difficulties on using a proper syntax and building a project structure that does not violate SOLID principles. 


## Main Challenges
When I first started this project, I didn’t know much about JavaScript.  
Since JavaScript is mainly used to update HTML, I thought I could do everything with Thymeleaf instead.  
Thymeleaf lets you insert data into HTML on the server side, so I believed it was enough.

At the beginning, things worked fine without JavaScript.  
I used Spring Boot and Thymeleaf to build the website, and I could show different content depending on the user’s request.  
But later, I started to face some problems.

One problem was that HTML forms only support GET and POST methods.  
I couldn’t easily use PUT or DELETE, which are important for RESTful APIs.  
Another problem was that the whole page reloaded every time the user clicked something.    
This made the website slower and less modern.

Also, it was hard to make dynamic features like filtering comments or editing posts without refreshing the whole page.  
I realized that this would be very difficult without using JavaScript.

## Solution
So I changed the project to use client-side rendering (CSR).  
Now, the backend is only for the REST API, and the frontend uses static HTML and JavaScript.  
The browser fetches the data and shows it without reloading the page.  
This makes the website more interactive and better for users.

## Key Learnings
Even though the change to Client-side rendering took me a while, it was a valuable experience for me to learn the significance of project planning and Javascript, the meaning of RESTful api, and the difference between Server-side and Client-side rendering.    

### Comparison: Server-side vs Client-side Rendering

| Criteria                    | Server-side Rendering (SSR)                        | Client-side Rendering (CSR)                          |
|----------------------------|----------------------------------------------------|------------------------------------------------------|
| Initial Load Speed         | Faster initial load                                | Slower initial load due to JavaScript parsing        |
| SEO                        | Better for SEO                                     | Worse for SEO unless using SSR frameworks            |
| Page Navigation            | Full-page reload required                          | Smooth, partial updates with no full reload          |
| RESTful API Compatibility  | Harder to fully implement (limited to GET/POST)    | Fully compatible with RESTful API (GET/POST/PUT/DEL) |
| JavaScript Requirement     | Not required                                       | Required for rendering and interaction               |
| Complexity of UI Features  | Hard to implement dynamic features (e.g., filtering) | Easier to support rich interactions and UI updates |
| Server Load                | Higher (HTML rendering done on server)             | Lower (server sends only data, not HTML)             |
