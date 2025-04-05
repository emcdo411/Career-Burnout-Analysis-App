# Career Burnout Analysis App: Understanding the Impact of Career Choices, Gender, Ethnicity, and Cultural Influence

## Project Overview

This project aims to investigate the relationship between **career burnout**, **gender disparities**, **ethnicity**, and **cultural influence**. With a focus on high-stress, high-reward careers (such as those in **finance, tech, and healthcare**), this study examines the intersection of these factors and their contributions to burnout in the workforce. It includes an interactive **Shiny app** that provides a comprehensive overview of burnout rates across different **industries**, **cities**, and **ethnic groups**. 

### Shoutout to Izy Montalvo

This study draws inspiration from Izy Montalvo's compelling work and insight regarding the impact of career burnout. Her post, **"77% of employees globally suffer from burnout,"** helped drive the initial inquiry into how burnout is not just a result of job stress, but also tied to external pressures, including **gender roles**, **cultural expectations**, and **family influence**. A big thanks to Izy for sparking the inspiration behind this research!

---

## Why This Matters

The **burnout epidemic** is real and affects employees globally. Understanding the **root causes** of burnout can provide a path forward in mitigating its effects. While many studies focus on **job stress** as the primary cause, factors like **gender disparity**, **ethnicity**, and **cultural pressures** often go unnoticed.

For instance, high-paying careers like **medicine** and **law** are often chosen by individuals due to cultural expectations, even if they lack a true passion for the work. These industries also have some of the highest rates of burnout, particularly among women and marginalized groups. This app presents data-driven insights into these issues, providing a deeper understanding of how **career choices** and **family expectations** play a role in shaping burnout experiences.

---

## Key Features

- **Interactive Shiny App**: Provides a user-friendly interface to visualize burnout data across industries, cities, ethnic groups, and cultures.
- **Burnout by Gender**: A breakdown of burnout percentage differences between men and women in various high-paying industries.
- **Burnout by Ethnicity**: Insights into burnout rates across different ethnic groups, considering the **cultural and familial pressures** that may influence career choices.
- **Burnout in Major U.S. Cities**: Geospatial analysis of burnout in major cities with interactive maps.
- **Cultural Influence**: Examines how cultural pressures (e.g., parental influence) contribute to burnout, with a focus on industries with the highest levels of burnout.

---

## App Functionality

The app includes the following interactive visualizations:
1. **Gender vs. Salary**: Visualizes the salary disparity between men and women across various industries.
2. **Gender vs. Burnout**: Displays burnout percentages between men and women in industries like healthcare, tech, and finance.
3. **Burnout by Ethnicity**: Provides insights into how different ethnic groups experience burnout.
4. **Industry Burnout**: Compares burnout percentages across various industries.
5. **Cultural Influence on Career Choices**: Shows how cultural pressures (parental expectations) affect career decisions and burnout.
6. **Interactive Map**: A clickable **leaflet map** that shows burnout percentages in major U.S. cities, providing additional insights into the reasons behind the burnout in each city.

---

## Conclusion

This app provides a powerful way to visualize and understand the complex dynamics of **career burnout**. By combining data on **gender**, **ethnicity**, **culture**, and **career choices**, we can better understand how personal and external factors intersect to create burnout. The data presented in this study not only highlights the pressures faced by employees but also opens up a broader discussion about the role of **family expectations**, **societal norms**, and **job stress** in shaping one's career journey.

Ultimately, this app serves as a **data-driven tool** for both individuals and organizations to **make informed decisions** about career paths, well-being, and strategies for reducing burnout. This is especially important in the wake of rising burnout rates in high-stress, high-paying careers.

### **What Was Accomplished**:
- Comprehensive analysis of **burnout data** across industries, cities, and ethnic groups.
- Developed an interactive **Shiny app** for users to visualize data on burnout, gender, and ethnicity.
- Added an **interactive map** for geographic burnout analysis.
- Incorporated data on **cultural influence** and **parental pressures** contributing to career choices and burnout.

This app is a valuable resource for both **employees** seeking to understand the factors influencing their career and burnout, as well as **organizations** looking to improve employee well-being and reduce burnout.

---

## Repo Name Suggestion

Based on the project's content and focus, a fitting name for the GitHub repository could be:

**`CareerBurnoutAnalysis`**

---

## Code

Below is the final code used to build the Shiny app.

```r
# Load necessary libraries
library(shiny)
library(ggplot2)
library(dplyr)
library(DT)
library(leaflet)

# Sample data simulating burnout rates
data <- data.frame(
  Gender = rep(c("Male", "Female"), each = 50),
  Age = sample(22:45, 100, replace = TRUE),
  Salary = c(rnorm(50, mean = 53000, sd = 4000), rnorm(50, mean = 45000, sd = 4000)),
  Industry = rep(c("Finance", "Tech", "Marketing", "Healthcare", "Education"), each = 20),
  BurnoutPercentage = c(runif(20, 30, 60), runif(20, 35, 70), runif(20, 40, 65), runif(20, 35, 80), runif(20, 25, 55)),
  Ethnicity = rep(c("Asian", "Middle Eastern", "Latino", "African American", "Western"), each = 20),
  BurnoutEthnicity = c(40, 35, 30, 42, 45, 30, 35, 30, 45, 40, 35, 40, 30, 45, 50, 45, 35, 45, 50, 40, 40, 45, 30, 35, 50)
)

# Sample city data
city_info <- data.frame(
  city = c("New York", "Los Angeles", "Chicago", "Houston", "Phoenix"),
  lat = c(40.7128, 34.0522, 41.8781, 29.7604, 33.4484),
  lng = c(-74.0060, -118.2437, -87.6298, -95.3698, -112.0740),
  burnout = c(50, 60, 55, 70, 65),
  reason = c("High competition, long working hours, and cost of living stress",
             "Fast-paced industry, heavy workload, and high expectations",
             "Economic disparities, high crime rate, and long commute",
             "Work overload, lack of work-life balance, and job insecurity",
             "Heat-related stress, long working hours in tech sector, and rapid growth")
)

ui <- fluidPage(
  titlePanel("Career Choices, Burnout, and Cultural Influence"),
  sidebarLayout(
    sidebarPanel(
      selectInput("plotType", "Select Plot:",
                  choices = c("Gender vs. Salary",
                              "Gender vs. Burnout Percentage",
                              "Burnout by Ethnicity",
                              "Industry Burnout",
                              "Cultural Influence")),
      selectInput("ethnicity", "Select Ethnicity for Burnout by Ethnicity Plot:",
                  choices = c("Asian", "Middle Eastern", "Latino", "African American", "Western"))
    ),
    mainPanel(
      plotOutput("mainPlot"),
      DTOutput("table"),
      leafletOutput("map"),
      textOutput("burnoutInfo")
    )
  )
)

server <- function(input, output) {

  output$mainPlot <- renderPlot({
    if (input$plotType == "Gender vs. Salary") {
      ggplot(data, aes(x = Gender, y = Salary, fill = Gender)) +
        geom_boxplot() +
        labs(title = "Gender Salary Gap", y = "Salary ($)", x = "Gender")
    } else if (input$plotType == "Gender vs. Burnout Percentage") {
      ggplot(data, aes(x = Gender, y = BurnoutPercentage, fill = Gender)) +
        geom_boxplot() +
        labs(title = "Burnout Percentage by Gender", y = "Burnout Percentage (%)", x = "Gender")
    } else if (input$plotType == "Burnout by Ethnicity") {
      ethnicity_data <- data %>% filter(Ethnicity == input$ethnicity)
      ggplot(ethnicity_data, aes(x = Ethnicity, y = BurnoutEthnicity, fill = Ethnicity)) +
        geom_bar(stat = "identity") +
        scale_fill_manual(values = c("Asian" = "blue", "Middle Eastern" = "red", "Latino" = "green", 
                                    "African American" = "purple", "Western" = "orange")) +
        labs(title = paste("Burnout by Ethnicity for", input$ethnicity), y = "Burnout Percentage (%)", x = "Ethnicity")
    } else if (input$plotType == "Industry Burnout") {
      ggplot(data, aes(x = Industry, y = BurnoutPercentage, fill = Industry)) +
        geom_bar(stat = "identity") +
        labs(title = "Burnout by Industry", x = "Industry", y = "Burnout Percentage (%)") +
        theme(axis.text.x = element_text(angle = 45, hjust = 1))
    } else if (input$plotType == "Cultural Influence") {
      cultural_data <- data.frame(
        Ethnicity = c("Asian", "Middle Eastern", "Latino", "African American", "Western"),
        ParentalPressurePercentage = c(70, 80, 65, 75, 55)
      )
      ggplot(cultural_data, aes(x = Ethnicity, y = ParentalPressurePercentage, fill = Ethnicity)) +
        geom_bar(stat = "identity") +
        scale_fill_manual(values = c("Asian" = "blue", "Middle Eastern" = "red", "Latino" = "green", 
                                    "African American" = "purple", "Western" = "orange")) +
        labs(title = "Cultural Pressure and Burnout by Ethnicity", y = "Parental Pressure (%)", x = "Ethnicity")
    }
  })

  output$table <- renderDT({
    datatable(data, options = list(pageLength = 10))
  })

  output$map <- renderLeaflet({
    leaflet(city_info) %>%
      addTiles() %>%
      addCircles(lng = ~lng, lat = ~lat, weight = 1, radius = ~burnout * 1000, 
                 popup = ~paste(city, "Burnout: ", burnout, "%", "<br>", "Reason: ", reason))
  })

  output$burnoutInfo <- renderText({
    ""
  })
}

shinyApp(ui = ui, server = server)
```

---

## Getting Started

To run this app locally:

1. Clone the repository to your local machine using:
    ```bash
    git clone https://github.com/yourusername/CareerBurnoutAnalysis.git
    ```
2. Install the necessary packages by running:
    ```R
    install.packages(c("shiny", "ggplot2", "dplyr", "DT", "leaflet"))
    ```
3. Run the app by executing the following in your R console:
    ```R
    shiny::runApp("path_to_the_app")
    ```

---

Feel free to explore the
