---
title: Why Code Conventions Matter
date: '2024-12-16'
author: Ezra Natanael
categories:
  - Misc
slug: why-code-conventions-matter
toc: true
---

## Introduction

In my learning journey over the past 2 years, after tackling various projects and internships, one lesson I’ve carried with me is this: **consistency in code matters**. Whether I was building scalable RESTful APIs, integrating third-party services, or working with distributed systems, I realized that adhering to a code convention not only made my projects clean and readable but also improved collaboration with teammates.

Early on, I didn’t pay much attention to coding style - what mattered to me was making it “work.” But as projects grew larger, bugs harder to track, and teammates more involved, I saw firsthand how inconsistent code slows you down. Naming confusion, messy formatting, or inconsistent structures caused hours of frustration.

This is why companies like **Google**, **Uber**, and **Airbnb** have published official **style guides** - their engineers rely on consistent conventions to maintain clean and reliable codebases. In this article, I’ll discuss why code conventions are critical and provide examples in Go, along with references to popular style guides.

---

## Why Do We Need Code Convention?

1. Improves Readability

Code conventions ensure that the code remains clean and understandable, even if someone new joins the team. It allows developers to focus on the logic rather than figuring out inconsistent naming, indentations, or patterns.

2. Enhances Maintainability

Projects grow over time. Code written without a standard often becomes a nightmare to maintain. Conventions ensure that returning to an old codebase (or passing it to someone else) remains straightforward and pain-free.

3. Facilitates Collaboration

When multiple developers work on the same project, consistent conventions reduce friction. Everyone follows the same “rules,” making code reviews easier and encouraging a productive team dynamic.

4. Reduces Bug

Inconsistent code structure can hide logical bugs. When conventions are applied, anomalies or typos stand out more clearly.

5. Aligned with Industry Practices
   
Companies like **Google** and **Airbnb** enforce style guides for their teams. Adhering to these standards aligns your code with industry expectations, improving your professional coding habits.

---

## Code Conventions In Action (Go Example)

Let’s see an example to illustrate how conventions impact code quality.

**Messy Code (Without Conventions)**

```go
package main

import (
    "fmt"
    "net/http"
    "encoding/json"
)

type responseStruct struct {
    Message string
    CODE int
}

func apiHandler(w http.ResponseWriter, r *http.Request){
    rs := responseStruct{
        Message: "Hello world",
        CODE:200}
    w.Header().Set("content-type", "application/json")
    json.NewEncoder(w).Encode(rs)
    fmt.Println("API is hit")
}

func main(){
    http.HandleFunc("/api", apiHandler)
    fmt.Println("Server running")
    http.ListenAndServe(":8080", nil)
}
```

**Clean Code (Following Conventions)**

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
)

// Response defines the structure for API responses.
type Response struct {
    Message string `json:"message"`
    Code    int    `json:"code"`
}

// APIHandler handles the /api endpoint.
func APIHandler(w http.ResponseWriter, r *http.Request) {
    response := Response{
    	Message: "Hello, World!",
    	Code:    http.StatusOK,
    }

    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(response)

    fmt.Println("INFO: /api endpoint was accessed successfully")
}

func main() {
    http.HandleFunc("/api", APIHandler)

    fmt.Println("Server is running on http://localhost:8080")
    err := http.ListenAndServe(":8080", nil)
    if err != nil {
    	fmt.Println("ERROR:", err)
    }
}
```

### Key Improvements

- **Consistent Naming:**
  - `APIHandler` follows **PascalCase** as per Go’s convention for exported functions.
  - `Response` struct fields are in **camelCase**.
- **Readable Code:**
  - Proper spacing and indentation improve readability.
- **Header Consistency:**
  - `"Content-Type"` follows Go’s idiomatic style.
- **Clear Logs:**
  - Logs are informative (e.g., “INFO” and “ERROR”) instead of generic.

---

## Industry Example (Style Guides)

Major tech companies emphasize code conventions and publish style guides for their developers. Here are a few examples:

1. **Google Go Style Guide**: Google follows strict conventions for naming, comments, error handling, and formatting. For instance:

    - Use **camelCase** for variables and **PascalCase** for structs and exported functions.
    - Avoid abbreviations and prefer meaningful names.

    References: [Google Go Style Guide](https://google.github.io/styleguide/go/)

2. **Uber Go Style Guide**:
   Uber’s guide focuses on idiomatic Go conventions to ensure clean, scalable code. Key rules include:

	- Prefer **explicitness** over magic or clever tricks.
	- Always handle errors explicitly.

    References: [Uber Go Style Guide](https://github.com/uber-go/guide/blob/master/style.md)

3. **Airbnb JavaScript Style Guide**:
   While not for Go, Airbnb's popular style guide has influenced coding conventions across languages, it stresses:

   - Consistent naming.
   - Proper whitespace and indentation.
   - Avoiding global variables.
  
    References: [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)

---

## Closing Thoughts

After working on multiple projects, I’ve learned that code conventions are not just about aesthetics - they’re about building clean, maintainable, and scalable systems. Whether you’re writing APIs in Go, managing complex systems, or collaborating with a team, consistent conventions help you write code that’s easy to understand, debug, and extend.

Take a cue from industry giants like Google and Uber. They enforce style guides for a reason: consistency is key to success. Tools like **`gofmt`** and linters like **`golangci-lint`** can help enforce conventions in your Go projects automatically.

So the next time you start a new project, establish a clear style guide early. Your teammates - and your future self - will thank you for it!
