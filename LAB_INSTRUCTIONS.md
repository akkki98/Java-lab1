# Lab: Create Code by Using GitHub Copilot Autocompletions - Java Spring Boot

## 📋 Overview

In this hands-on lab, you'll use **GitHub Copilot** to build a Quarterly Sales Report API using Java Spring Boot. You'll learn how to leverage AI-assisted coding to accelerate development and generate business logic from natural language comments.

### What You'll Learn

- ✅ Generate code from comments using GitHub Copilot
- ✅ Use code line completions for business logic
- ✅ Create REST APIs with Spring Boot
- ✅ Build data models and services with minimal typing
- ✅ Implement quarterly sales reporting functionality

### Estimated Time

⏱️ **45-60 minutes**

---

## 🎯 Problem Statement

**ABC Retail Corporation** needs a REST API to generate quarterly sales reports. The finance team currently spends days manually aggregating sales data from 10 departments and 100+ products. Your task is to build an API that:

- Generates sample sales data (date, department, product, quantity, price)
- Calculates total sales revenue by quarter (Q1-Q4)
- Returns results in JSON format for dashboard integration

**Formula:** `Total Sales = Quantity Sold × Unit Price`

---

## 📦 Prerequisites

Before starting this lab, ensure you have:

- ✅ **Visual Studio Code** installed
- ✅ **Java Development Kit (JDK 17 or later)**
- ✅ **Maven** (or Gradle)
- ✅ **Spring Boot Extension Pack** for VS Code
- ✅ **GitHub Copilot** extension (active subscription required)
- ✅ **GitHub Copilot Chat** extension

### Verify Prerequisites

```bash
# Check Java version
java -version

# Check Maven version
mvn -version
```

---

## 🚀 Lab Instructions

### Part 1: Create a Spring Boot Application

1. **Open Visual Studio Code**

2. **Open the Chat view** 
   - Press `Ctrl+Alt+I` (Windows/Linux) or `Cmd+Alt+I` (Mac)
   - Or open Command Palette (`Ctrl+Shift+P`) and select "GitHub Copilot: Open Chat"

3. **In the Chat view, enter the following prompt:**

   ```
   @workspace /new Spring Boot application named sales-report-api. Use Java 17, Spring Boot 3.2, Spring Web, and Lombok. Create a Maven project structure.
   ```

4. **Review Copilot's suggestions** and select **"Create Workspace"**

5. **Choose your Desktop folder** (or preferred location) as the parent directory

6. **When prompted, select "Open"** to open the new project in VS Code

7. **Wait for dependencies to download** (you'll see Maven/Gradle activity in the status bar)

> 💡 **Tip:** If Copilot suggests creating a `pom.xml`, review it to ensure it includes Spring Web and Lombok dependencies.

---

### Part 2: Create the Sales Data Model

1. **In the `src/main/java` directory, create a new package:**
   - Right-click on your base package → New → Package
   - Name it: `com.demo.salesreport.model`

2. **Create a new Java file** in the `model` package:
   - Right-click on `model` → New → Java Class
   - Name it: `SalesData.java`

3. **Add the following comment at the top of the file:**

   ```java
   package com.demo.salesreport.model;
   
   import lombok.Data;
   import lombok.AllArgsConstructor;
   import lombok.NoArgsConstructor;
   import java.time.LocalDate;
   
   // Create a SalesData class with fields: dateSold (LocalDate), departmentName (String), productId (Integer), quantitySold (Integer), unitPrice (Double)
   ```

4. **Press Enter** and let GitHub Copilot suggest the class structure

5. **Review the suggestion** (it should look similar to this):

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class SalesData {
       private LocalDate dateSold;
       private String departmentName;
       private Integer productId;
       private Integer quantitySold;
       private Double unitPrice;
   }
   ```

6. **Accept the suggestion** by pressing `Tab` or clicking "Accept"

7. **Save the file** (`Ctrl+S`)

> 🎯 **Key Learning:** Copilot understands natural language comments and can generate complete class structures with annotations.

---

### Part 3: Create the Sales Data Service

1. **Create a new package** called `service`:
   - Right-click on base package → New → Package
   - Name: `com.demo.salesreport.service`

2. **Create a new Java file:**
   - Name: `SalesDataService.java`

3. **Add the following code structure with a comment:**

   ```java
   package com.demo.salesreport.service;
   
   import com.demo.salesreport.model.SalesData;
   import org.springframework.stereotype.Service;
   import java.time.LocalDate;
   import java.util.List;
   import java.util.Random;
   import java.util.stream.Collectors;
   import java.util.stream.IntStream;
   
   @Service
   public class SalesDataService {
   
       // Method to generate 1000 random SalesData records with dates in 2023, random departments 1-10, random product IDs 1-100, random quantity 1-100, and random unit price 0-100
   ```

4. **Press Enter** and wait for Copilot to suggest the method

5. **Review and accept the suggestion** (should be similar to):

   ```java
   public List<SalesData> generateSalesData() {
       Random random = new Random();
       return IntStream.range(0, 1000)
               .mapToObj(i -> new SalesData(
                       LocalDate.of(2023, random.nextInt(12) + 1, random.nextInt(28) + 1),
                       "Department " + (random.nextInt(10) + 1),
                       random.nextInt(100) + 1,
                       random.nextInt(100) + 1,
                       random.nextDouble() * 100
               ))
               .collect(Collectors.toList());
   }
   ```

6. **Save the file**

> 🎯 **Key Learning:** Copilot can generate complex stream operations and lambda expressions from descriptive comments.

---

### Part 4: Create the Quarterly Report Service

1. **In the `service` package, create a new file:**
   - Name: `QuarterlySalesService.java`

2. **Add the service annotation and comment:**

   ```java
   package com.demo.salesreport.service;
   
   import com.demo.salesreport.model.SalesData;
   import org.springframework.stereotype.Service;
   import java.util.List;
   import java.util.Map;
   import java.util.stream.Collectors;
   
   @Service
   public class QuarterlySalesService {
   
       // Method to generate a quarterly sales report from a list of SalesData. Calculate total sales (quantitySold * unitPrice) grouped by quarter (Q1, Q2, Q3, Q4)
   ```

3. **Press Enter** and let Copilot suggest the method signature and body

4. **Accept the suggestion** (should include a method like):

   ```java
   public Map<String, Double> generateQuarterlySalesReport(List<SalesData> salesDataList) {
       return salesDataList.stream()
               .collect(Collectors.groupingBy(
                       data -> getQuarter(data.getDateSold().getMonthValue()),
                       Collectors.summingDouble(data -> 
                           data.getQuantitySold() * data.getUnitPrice())
               ));
   }
   ```

5. **Add another comment for the helper method:**

   ```java
   // Helper method to determine quarter from month number (1-3: Q1, 4-6: Q2, 7-9: Q3, 10-12: Q4)
   ```

6. **Let Copilot generate the helper method:**

   ```java
   private String getQuarter(int month) {
       if (month >= 1 && month <= 3) return "Q1";
       if (month >= 4 && month <= 6) return "Q2";
       if (month >= 7 && month <= 9) return "Q3";
       return "Q4";
   }
   ```

7. **Save the file**

> 🎯 **Key Learning:** Copilot understands business logic and can generate grouping and aggregation operations.

---

### Part 5: Create the REST Controller

1. **Create a new package** called `controller`:
   - Name: `com.demo.salesreport.controller`

2. **Create a new Java file:**
   - Name: `SalesReportController.java`

3. **Add the controller structure with comments:**

   ```java
   package com.demo.salesreport.controller;
   
   import com.demo.salesreport.service.QuarterlySalesService;
   import com.demo.salesreport.service.SalesDataService;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import java.util.Map;
   
   @RestController
   @RequestMapping("/api/sales")
   public class SalesReportController {
   
       // Constructor injection for SalesDataService and QuarterlySalesService
   ```

4. **Press Enter** and accept Copilot's constructor suggestion:

   ```java
   private final SalesDataService salesDataService;
   private final QuarterlySalesService quarterlySalesService;
   
   public SalesReportController(SalesDataService salesDataService, 
                                QuarterlySalesService quarterlySalesService) {
       this.salesDataService = salesDataService;
       this.quarterlySalesService = quarterlySalesService;
   }
   ```

5. **Add a comment for the endpoint:**

   ```java
   // GET endpoint at /quarterly-report that generates sales data and returns quarterly sales report as JSON
   ```

6. **Let Copilot generate the endpoint:**

   ```java
   @GetMapping("/quarterly-report")
   public Map<String, Double> getQuarterlyReport() {
       var salesData = salesDataService.generateSalesData();
       return quarterlySalesService.generateQuarterlySalesReport(salesData);
   }
   ```

7. **Save the file**

> 🎯 **Key Learning:** Copilot understands Spring Boot annotations and REST API patterns.

---

### Part 6: Create the Main Application Class

1. **Ensure you have the main application class** in your base package:
   - Name: `SalesReportApplication.java`

2. **If not present, create it with this comment:**

   ```java
   package com.demo.salesreport;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   // Spring Boot main application class
   ```

3. **Let Copilot complete it:**

   ```java
   @SpringBootApplication
   public class SalesReportApplication {
       public static void main(String[] args) {
           SpringApplication.run(SalesReportApplication.class, args);
       }
   }
   ```

---

### Part 7: Configure Application Properties (Optional)

1. **Open or create** `src/main/resources/application.properties`

2. **Add basic configuration:**

   ```properties
   # Application name
   spring.application.name=sales-report-api
   
   # Server port
   server.port=8080
   
   # Logging level
   logging.level.com.demo.salesreport=INFO
   ```

---

### Part 8: Run and Test the Application

1. **Open the Terminal** in VS Code:
   - Go to `Terminal` → `New Terminal`
   - Or press `` Ctrl+` ``

2. **Run the application using Maven:**

   ```powershell
   ./mvnw spring-boot:run
   ```

   **Or if using Gradle:**

   ```powershell
   ./gradlew bootRun
   ```

   **Or on Windows without wrapper:**

   ```powershell
   mvn spring-boot:run
   ```

3. **Wait for the application to start**. You should see output like:

   ```
   Started SalesReportApplication in 3.456 seconds
   ```

4. **Test the endpoint** using one of these methods:

   **Option A: Using Browser**
   - Open: `http://localhost:8080/api/sales/quarterly-report`

   **Option B: Using PowerShell**
   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/sales/quarterly-report | Select-Object -ExpandProperty Content
   ```

   **Option C: Using curl (if installed)**
   ```bash
   curl http://localhost:8080/api/sales/quarterly-report
   ```

5. **Verify the JSON response:**

   ```json
   {
       "Q1": 642769.27,
       "Q2": 667269.19,
       "Q3": 635637.50,
       "Q4": 672247.31
   }
   ```

   *(Note: Your values will differ due to random data generation)*

6. **Stop the application** by pressing `Ctrl+C` in the terminal

---

## 🎉 Congratulations!

You've successfully completed the lab! You used GitHub Copilot to:

- ✅ Create a complete Spring Boot application structure
- ✅ Generate data models with Lombok annotations
- ✅ Implement service layer logic from comments
- ✅ Create REST API endpoints with proper dependency injection
- ✅ Generate complex business logic with minimal manual coding

---

## 📊 Project Structure

Your final project structure should look like:

```
sales-report-api/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/demo/salesreport/
│   │   │       ├── SalesReportApplication.java
│   │   │       ├── controller/
│   │   │       │   └── SalesReportController.java
│   │   │       ├── model/
│   │   │       │   └── SalesData.java
│   │   │       └── service/
│   │   │           ├── SalesDataService.java
│   │   │           └── QuarterlySalesService.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/
└── pom.xml
```

---

## 🔍 Key Takeaways

### What You Learned

1. **Comment-Driven Development**: Use descriptive comments to guide Copilot suggestions
2. **Context Awareness**: Copilot uses surrounding code and imports to provide relevant completions
3. **Spring Boot Patterns**: Copilot understands framework-specific annotations and patterns
4. **Code Quality**: Always review and test Copilot-generated code
5. **Productivity Boost**: AI-assisted coding significantly reduces boilerplate and repetitive tasks

### Best Practices When Using Copilot

- ✅ Write clear, descriptive comments
- ✅ Provide good context (imports, class structure)
- ✅ Review suggestions before accepting
- ✅ Test generated code thoroughly
- ✅ Use consistent naming conventions
- ✅ Break complex tasks into smaller steps

---

## 🚀 Bonus Challenges

Want to extend your learning? Try these challenges:

### Challenge 1: Add Filtering
- Add a query parameter to filter by department name
- Example: `/api/sales/quarterly-report?department=Department 5`

### Challenge 2: Add Date Range
- Add optional start and end date parameters
- Filter sales data by date range

### Challenge 3: Calculate Averages
- Add an endpoint that returns average sales per quarter
- Endpoint: `/api/sales/quarterly-average`

### Challenge 4: Top Performer
- Identify and return the top-performing department per quarter
- Endpoint: `/api/sales/top-departments`

### Challenge 5: Unit Tests
- Use Copilot to generate unit tests for your services
- Test data generation and quarterly calculations

---

## 🐛 Troubleshooting

### Common Issues

**Issue: Application won't start**
- ✅ Check Java version: `java -version` (must be 17+)
- ✅ Verify Maven installation: `mvn -version`
- ✅ Check port 8080 isn't in use
- ✅ Review console for error messages

**Issue: Copilot not suggesting code**
- ✅ Verify Copilot subscription is active
- ✅ Check Copilot icon in VS Code status bar
- ✅ Try reloading VS Code window
- ✅ Ensure file has proper extension (.java)

**Issue: Lombok not working**
- ✅ Verify Lombok dependency in `pom.xml`
- ✅ Install Lombok plugin for your IDE
- ✅ Enable annotation processing in IDE settings

**Issue: JSON response is empty**
- ✅ Check service methods are annotated with `@Service`
- ✅ Verify controller mapping annotations
- ✅ Review console logs for errors

---

## 📚 Additional Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Project Lombok](https://projectlombok.org/)
- [Java Stream API Guide](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)

---

## 📝 Feedback

Have suggestions for improving this lab? Found an issue?

- Open an issue on this repository
- Submit a pull request with improvements
- Share your experience with GitHub Copilot

---

## 📄 License

This lab is provided under the MIT License. Feel free to use and modify for educational purposes.

---

**Happy Coding with GitHub Copilot! 🚀**
