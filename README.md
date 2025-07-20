# Beyond the Surface:
#### *Leveraging Machine Learning to Monitor Rural and Urban Well Functionality in Tanzania*
![hands-reaching-for-clean-drinking-water](https://github.com/user-attachments/assets/2910fdbc-2c82-4db8-87db-72cf957732dc)

## **Business Understanding**
### **Background**
In a nation where one third of the country is arid to semi-arid, access to basic water has been a constant challenge for the longest time. Despite the government's effort to deal with this issue, Tanzania has remained a water-stressed country.

Other than the three major lakes in the region, ground water has been the major source of water for the nation's people. It is at the back of this that in 2006, the Government of Tanzania adopted a National Water Sector Development Strategy that aimed to promote integrated water resources management and the development of urban and rural water supply.

As part of its National Water Sector Development Strategy, the Tanzanian government prioritized decentralized water supply infrastructure, with a strong emphasis on constructing wells and boreholes throughout the country. As much as it has made significant progress improving the access of thousands of citizens, it still has a long way to go.

---

### **Project Overview**
As of 2023, only 61% of households in Tanzania have access to a basic water-supply. The Tanzanian government has made efforts to bridge this gap, however, it has been a really slow descent. 

This project seeks to:
- Investigate why the percentage of households that have access to basic water is slightly above 50%, despite the National Water Sector Development Strategy being at work for almost 2 decades.

- Leverage supervised machine learning to help the Government keep track of the functionality status of the wells across the country, whether functional, needs repair or non-functional.
 
- Provide recommendations to help the Tanzanian Government accelerate their National Water Sector Development Strategy.

---

### **Project Objectives**
This project seeks to:
- Develop a machine learning model that predicts whether a well is:
    - *Functional*

    - *Non-functional*

    - *Functional but needs repair*

    from features like construction year, installer, funder, and geolocation data.

- Achieve a target classification accuracy of **at least 85%** and an F1-score of **at least 85%**.

- Identify key features (e.g., what kind of pump is operating, when it was installed, how it is managed) that drive well functionality.

- Support National Water Sector Development Strategy by identifying underperforming or non-functional wells using data science tools.

- Develop a blueprint system that can be adapted for similar water access initiatives globally.

---

### **StakeHolders**
- *The Tanzanian Government*: By predicting which wells are functional, non functional or need repairs, the government can have a clear idea of where resources are needed the most and help them drive their Water Development Strategy even further and improve the country's water situation. 

- *External Donors*: These are individuals or institutions who provide resources to help the nation.

- *Non-Profit Organizations*: These are organizations looking to help.

- *Local Communities*: These are the people the wells help directly. 

---

## **Data Understanding**
This project required a dataset that represents Tanzania's water systems updated and created by people who manage the said systems.

The data used in this project was sourced from [DrivenData's Tanzania Water Pumps competition](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/). Who in turn sourced it from Taarifa, an open source platform that aggregates data from the Tanzania Ministry of Water and the Tanzanian Ministry of Water.

This repository contains 3 dataset:
- `Training_Set_Values.csv` - Contains features/predictors. (59400 rows, 40 columns)

- `Training_Set_Labels.csv` - Contains target variables. (59400 rows, 40 columns)

- `Test_Set_Values` - Contains test values for testing for generalization. (14,850 records)

Sample columns include:

* `funder`, `installer`, `construction_year`
* `gps_height`, `longitude`, `latitude`
* `extraction_type`, `water_quality`, `quantity`
* `region`, `district_code`, `ward`, `subvillage`

---

## 🧹 Data Preparation

We followed best practices to prepare the data:

### 1. **Cleaning**

* Handled missing values
* Converted records to their correct data types and standardized text (e.g., multiple versions of "other")
* Dropped high-cardinality, redundant, or uninformative columns: `subvillage`, `recorded_by`, `scheme_name`, etc.

### 2. **Encoding**

* Categorical variables with:

  * **Ordinal Encoding** (if `<= 50` unique values)
  * **Frequency Encoding** (if `> 50` unique values)
* Stored encoders in a dictionary for consistent transformation of test set

### 3. **Class Balancing**

* Applied **SMOTE (Synthetic Minority Oversampling Technique)** to address imbalance in target labels

---

## 🤖 Modeling

We tested multiple models and tuned them iteratively:

### 1. **Baseline Model**

* Logistic Regression (performed poorly due to multi-class nature)

### 2. **Decision Tree Classifier**

* Tuned `max_depth`, `min_samples_leaf`, `criterion`
* Detected overfitting from training vs test score differences
* Used feature_importances_ to assess important features

### 3. **Random Forest Classifier** 

* Final model used `criterion='entropy'`
* Tuned using **RandomizedSearchCV**
* Achieved 88% training accuracy, 82% cross-validated accuracy

### Evaluation Metrics

* **Accuracy**: Primary metric for final selection
* **Precision, Recall, F1-Score**: Reported per class
* **Confusion Matrix**: Visualized and analyzed in detail

---

## ✅ Evaluation

### Final Model Performance

* Test Accuracy: \~77%
* Training Accuracy: \~88%
* No evidence of overfitting (evidence of class imbalance)

### Confusion Matrix Analysis

* Model performs best on majority class: `functional`
* Poor performance on under represented class

### Feature Importance

Top contributing features:

* `quantity`, `source_type`, `installer`, `region`
* `extraction_type`, `management`

---

## Insights & Recommendations
This section outlines key findings from the analysis and offers actionable recommendations for stakeholders, particularly the Tanzanian government and water sector agencies.

### Key Insights

##### Funders vs Wells Functionality
- Government-funded wells are the most numerous but also the least reliable.
Despite the government funding the highest number of wells across the country, it is also associated with the highest number of non-functional wells. This raises concerns about the quality of installation, oversight, or post-installation maintenance.

![image.png](attachment:image.png)

##### Installers vs Wells Functionality
- Wells installed by the District Water Engineer have significantly higher durability.
When examining functionality by installer, wells installed by the District Water Engineer exhibited the highest functionality rates, indicating that technical expertise and standardized procedures have a positive impact on sustainability. This suggests a potential gap in the skills or practices of other installers.

![image-3.png](attachment:image-3.png)

##### Extraction_Type vs Wells Functionality
- Gravity-fed wells show strong long-term performance.
Wells that utilize gravity as their extraction method show the highest rate of functionality compared to other extraction types such as hand pumps or motorized systems. This could be due to their mechanical simplicity and lower maintenance requirements.

![image-2.png](attachment:image-2.png)

##### Source vs Well Functionality
- Spring-based water sources are associated with high functionality.
Among all water source types, springs produced the most consistently functional wells. This suggests that in addition to improving installation quality, careful selection of the water source itself plays a critical role in the longevity of a well.

![image-4.png](attachment:image-4.png)

---

### Strategic Recommendations
Based on the insights above, we recommend the following actionable steps for improving rural water access sustainability:

1. Enhance government accountability in project execution.
Establish monitoring frameworks for government-funded wells to ensure installations follow quality assurance protocols and receive proper maintenance.

2. Standardize installations through certified professionals.
Require that all wells, especially those funded by public or NGO sources, be installed under the supervision of certified District Water Engineers.

3. Adopt gravity-based extraction systems wherever viable.
Given their durability and low maintenance, gravity-fed wells should be prioritized, particularly in regions where topography supports them.

4. Prioritize spring sources during site selection.
Water sourcing should be more deliberate. Feasibility studies should assess whether natural springs are available and accessible before selecting a site.

5. Invest in data quality and infrastructure management systems.
Many records lacked reliable information in fields such as permit and public_meeting. Better data collection can enhance future analysis and project planning

---

## 🛠 Technologies Used

* Python
* pandas
* scikit-learn, imbalanced-learn (SMOTE)
* seaborn, matplotlib
* Jupyter Notebook

---

## 💻 How to Run the Project

1. Clone the repository:

```bash
git clone https://github.com/Assesa44/water-well-status-prediction
cd water-well-status-prediction
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Launch the notebook:

```bash
jupyter notebook wells_analysis.ipynb
```

---

## **Project Structure**
```
Tanzania-Wells-Functionality-Predictions/
│
├── Data/
│   └── cleaned_Data/
│   └── Raw_Data/
|   └── Metadata.txt/
|
├── Images/
|
├── Notebooks/
│   └── wells_analysis.ipynb
│
├── .gitignore
│   
├── Tableau/
│   └── final_dashboard.twbx
│
├── Presentation/
│   └── insights_deck.pptx
│
├── README.md
|
└── requirements.txt
```

---

## 👩🏾‍💻 Author

**Vanessa Sandra Assesa**
Data Science Student, Moringa School

* Email: [veesandra30@gmail.com](mailto:veesandra30@gmail.com)
* GitHub: [@Assesa44](https://github.com/Assesa44)
* LinkedIn: [vanessa-sandra](https://linkedin.com/in/vanessa-sandra-7409b0320)
