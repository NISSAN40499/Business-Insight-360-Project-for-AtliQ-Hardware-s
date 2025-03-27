# User Acceptance Report

## Overview
For our project, we have created a User Acceptance Report that will be shared with the product owner, Nick Puri, as he is the main stakeholder and product owner. This document ensures that all aspects of the report meet the business and user requirements.

## User Acceptance Report Table
| Category                                      | Who?      | Validation Checklist                         | Approval?      | Comments |
|-----------------------------------------------|----------|--------------------------------------------|---------------|----------|
| **Power BI User Interface & Experience**      |          |                                            |               |          |
| PBI functionality                             |          | Visuals are clear and readable             | Under Review  |          |
|                                               |          | Dashboard provides actionable insights      | Under Review  |          |
|                                               |          | All functions are working as intended      | Under Review  |          |
| **Financial Numbers**                         |          |                                            |               |          |
| Transactional Data                            |          | Data is correct                            | Under Review  |          |
|                                               |          | Data is complete                           | Under Review  |          |
| **Sales/Marketing Numbers**                   |          |                                            |               |          |
| Transactional Data                            |          | Data is correct                            | Under Review  |          |
|                                               |          | Data is complete                           | Under Review  |          |
| **Forecast Numbers**                          |          |                                            |               |          |
| Transactional Data                            |          | Data is correct                            | Under Review  |          |
|                                               |          | Data is complete                           | Under Review  |          |
| **Region/Market / Customer Hierarchy**        |          | Hierarchy is correct                       | Under Review  |          |
| **Product Hierarchy**                         |          | Hierarchy is correct                       | Under Review  |          |

Approval Status Options: **Under Review**, **Done**, **Pending Review**

---
This is how Stakeholder Mapping Analysis Grid Looks like: https://drive.google.com/file/d/1NOO7unNMHw1Rkz0PuPNYsTXW9w091b7u/view?usp=sharing

## Stakeholder Mapping Analysis
Before collecting feedback from stakeholders, we conducted a Stakeholder Mapping Analysis. This helps us prepare for the stakeholder meeting by identifying different levels of power and interest within the project.

### Stakeholder Analysis Grid
- **Low Power, Low Interest**: These stakeholders need to be kept informed. They do not influence decisions but should be aware of project progress.
- **High Interest, Low Power**: These stakeholders should be shown consideration. Their power may grow in the future, and engaging them can be beneficial.
- **High Power, Low Interest**: These stakeholders must have their needs met efficiently. Their decisions impact the project, even if they are not deeply involved.
- **High Power, High Interest**: These stakeholders are key players. They require the most attention as they are influential decision-makers.

### Key Stakeholders
| Name                | Role                                      | Placement in Stakeholder Matrix             |
|---------------------|-----------------------------------------|---------------------------------------------|
| **T'Challa Tiwari** | Finance Operations Head                 | Low Interest, Low Power (Keep Informed)    |
| **Thor Hathodwala** | Supply Chain Expert                     | High Interest, Low Power (Show Consideration) |
| **Bruce Hariwali**  | Global Senior Sales Director            | High Power, Low Interest (Meet Their Needs) |
| **Carol Marvari**   | C-Level Executive                       | High Power, High Interest (Key Player)      |
| **Natasha Rao**     | Marketing Leader                        | High Power, High Interest (Key Player)      |

---

## Stakeholder Meeting Feedback Table
During the stakeholder meeting, we collected feedback to enhance the report. Managing and implementing these suggestions will improve the report's usability and impact.

| #  | Feedback Request                                 | Requester         | Action Owner |
|----|------------------------------------------------|-------------------|--------------|
| 1  | Report loads slow                              | Bruce Hariwali    |              |
| 2  | Add date of refresh, currency type, and values in Millions (USD) | T'Challa Tiwari  |              |
| 3  | Fix labels (SC Label)                          | T'Challa Tiwari  |              |
| 4  | 2022 Estimated Chart does not look correct    | T'Challa Tiwari  |              |
| 5  | Show GM %, not GM in sales and marketing chart | Nick Puri       |              |
| 6  | Show Targets                                  | Nick Puri       |              |
| 7  | Add button to switch between Targets & YoY comparison | Thor Hathodwala |              |
| 8  | Highlight Market / Customers by GM % Target Variance (Add a dynamic slicer) | Nick Puri | |
| 9  | Possible to switch between GM % and NP % in the marketing view? | Natasha Rao | |
| 10 | Show products per customer in one table      | Thor Hathodwala | |
| 11 | Show Market Share in Executive View          | Carol Marvari   | |
| 12 | Add drill-through or tooltip for a full trend over a table | Bruce Hariwali | |

The feedback collected is critical in refining our project and ensuring user satisfaction. Implementing these quick fixes and feature updates will make the report more impactful and valuable for the business.

---

## Additional Feature Requests
| Feedback from Stakeholder Meeting | Requester | Action Owner | Additional Hint |
|-----------------------------------|-----------|--------------|----------------|
| Add Target.xlsx file (attached below) to Power BI and connect it to the data model | Nick Puri | You |  |
| Create NS, GM %, and Net Profit % target measures and add them to finance view KPI card visuals | Nick Puri | You |  |
| Adjust labels as necessary | Nick Puri | You |  |
| Create a dynamic switch between two DAX measures | Thor Hathodwala | You | Google for 'create a toggle switch between DAX measures' |
| Display appropriate message if the target value returns blank | Thor Hathodwala | You |  |
| Adjust the P & L visuals' related measures to compare Target or LY based on selection | Thor Hathodwala | You | Use the same concept used to adjust KPI card visuals |

---

## Conclusion
This User Acceptance Report ensures that all functionalities, visualizations, and data validations align with stakeholder expectations. We will address all feedback points before final approval and delivery.

