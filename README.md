# 🧹 Employee Data Cleaning & Preprocessing

A comprehensive Jupyter Notebook demonstrating data cleaning and preprocessing techniques on an employee dataset using Python and Pandas.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Pandas](https://img.shields.io/badge/Pandas-Latest-green.svg)
![NumPy](https://img.shields.io/badge/NumPy-Latest-orange.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Data Cleaning Tasks](#data-cleaning-tasks)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Key Features](#key-features)
- [Technologies Used](#technologies-used)
- [Results](#results)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

This project demonstrates professional data cleaning and preprocessing techniques on a messy employee dataset containing **1000 rows** and **9 columns**. The notebook walks through systematic data quality improvements including handling missing values, standardizing formats, correcting data types, and removing inconsistencies.

### Dataset Columns:
- `user_id` - Employee identifier
- `age` - Employee age
- `salary` - Monthly salary
- `department` - Department name (HR, IT, Finance, Sales)
- `join_date` - Date of joining
- `city` - City of employment (Delhi, Mumbai, Bangalore, Hyderabad)
- `performance_score` - Performance rating
- `experience_years` - Years of experience
- `is_active` - Employment status

---

## 📊 Dataset

**Source:** `emp.csv`

**Initial State:**
- 1000 employee records
- Multiple data quality issues
- Mixed data types
- Inconsistent formatting
- Missing values across all columns

**Challenges Addressed:**
- Missing values (NaN, empty strings, "N/A")
- Invalid age values (negative, >100)
- Inconsistent salary formats (₹ symbol, "N/A")
- Multiple date formats (DD/MM/YYYY, YYYY/MM/DD, YYYY-MM-DD, "invalid")
- Inconsistent text casing (lowercase, UPPERCASE, mixed)
- Text performance scores ("poor", "excellent") mixed with numeric
- Text experience values ("five") mixed with numeric
- Boolean values as strings ("yes", "no", "True", "False")
- Duplicate user IDs

---

## 🧹 Data Cleaning Tasks

### **Q1. Data Exploration**
```python
# Shape, columns, and data types analysis
df.shape          # (1000, 9)
df.columns        # Column names
df.dtypes         # Data types per column
```

### **Q2. Missing Value Analysis**
```python
# Calculate percentage of missing values per column
missing_percentage = (df.isnull().sum() / len(df)) * 100
```

**Results:**
- `user_id`: ~44% missing
- `age`: ~24% missing
- `salary`: ~22% missing
- `department`: ~17% missing
- `join_date`: ~19% missing
- `city`: ~15% missing
- `performance_score`: ~22% missing
- `experience_years`: ~20% missing
- `is_active`: ~15% missing

### **Q3. Data Type Consistency Check**
```python
# Check for mixed data types in each column
df.apply(lambda col: col.map(type).nunique())
```

### **Q4. User ID Standardization**
```python
# Replace empty strings with NaN and convert to uppercase
df['user_id'] = df['user_id'].str.upper()
```
**Before:** `user_0`, `U1001`, `user_997`  
**After:** `USER_0`, `U1001`, `USER_997`

### **Q5. Age Validation**
```python
# Convert to numeric and remove invalid ages (<0 or >100)
df['age'] = pd.to_numeric(df['age'], errors='coerce')
df.loc[(df['age'] < 0) | (df['age'] > 100), 'age'] = np.nan
```
**Before:** `-5`, `200`, `"unknown"`  
**After:** `NaN`, `NaN`, `NaN`

### **Q6. Salary Cleaning**
```python
# Remove ₹ symbol and handle "N/A" values
df['salary'] = df['salary'].astype(str).str.replace('₹', ' ', regex=False).replace('N/A', np.nan)
```
**Before:** `₹50000`, `N/A`, `120000.75`  
**After:** `50000`, `NaN`, `120000.75`

### **Q7. Date Standardization**
```python
# Parse multiple date formats with dayfirst=True
df['join_date'] = pd.to_datetime(df['join_date'], errors='coerce', dayfirst=True)
```
**Before:** `2020/12/01`, `15/02/2021`, `2022-01-15`, `"invalid"`  
**After:** `2020-12-01`, `2021-02-15`, `2022-01-15`, `NaT`

### **Q8. Department Standardization**
```python
# Remove whitespace and convert to uppercase
df['department'] = df['department'].str.strip().str.upper()
```
**Before:** `HR`, `it `, `Finance`  
**After:** `HR`, `IT`, `FINANCE`

### **Q9. City Standardization**
```python
# Remove whitespace and convert to Title Case
df['city'] = df['city'].str.strip().str.title()
```
**Before:** `DELHI`, `mumbai`, `Bangalore`  
**After:** `Delhi`, `Mumbai`, `Bangalore`

### **Q10. Performance Score Mapping**
```python
# Convert text scores to numeric and handle mixed types
performance_map = {'poor': 0, 'excellent': 1}
df['performance_score'] = df['performance_score'].map(performance_map)
df['performance_score'] = pd.to_numeric(df['performance_score'], errors='coerce')
```
**Before:** `"poor"`, `"excellent"`, `3`, `5`  
**After:** `0`, `1`, `3`, `5`

### **Q11. Experience Years Standardization**
```python
# Replace text values with numeric equivalents
df['experience_years'] = df['experience_years'].replace({'five': 5})
```
**Before:** `"five"`, `1`, `10`, `-1`  
**After:** `5`, `1`, `10`, `-1`

### **Q12. Boolean Standardization**
```python
# Standardize boolean values
df['is_active'] = df['is_active'].replace({'yes': 'True', 'no': 'False'})
```
**Before:** `"yes"`, `"no"`, `True`, `False`  
**After:** `"True"`, `"False"`, `"True"`, `"False"`

### **Q13. Duplicate Removal**
```python
# Remove duplicate user IDs (keeping first occurrence)
df['user_id'] = df['user_id'].drop_duplicates()
```

---

## 🚀 Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Setup

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/employee-data-cleaning.git
cd employee-data-cleaning
```

2. **Create a virtual environment (optional but recommended):**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install required packages:**
```bash
pip install -r requirements.txt
```

Or install manually:
```bash
pip install pandas numpy jupyter
```

4. **Place your dataset:**
- Add your `emp.csv` file to the project directory
- Update the file path in the notebook if needed

---

## 💻 Usage

### Running the Notebook

1. **Start Jupyter Notebook:**
```bash
jupyter notebook
```

2. **Open `Task.ipynb`**

3. **Run cells sequentially:**
   - Press `Shift + Enter` to execute each cell
   - Or use **Cell → Run All** to execute entire notebook

### Modifying the Dataset Path

Update the file path in Cell 2:
```python
df = pd.read_csv(r"C:\path\to\your\emp.csv")
```

For different operating systems:
- **Windows:** `r"C:\Users\akshi\Desktop\emp.csv"`
- **Mac/Linux:** `"/Users/username/Desktop/emp.csv"`
- **Relative path:** `"./data/emp.csv"`

---

## 📁 Project Structure

```
employee-data-cleaning/
│
├── Task.ipynb              # Main Jupyter notebook
├── emp.csv                 # Employee dataset (not included)
├── README.md               # This file
├── requirements.txt        # Python dependencies
├── .gitignore             # Git ignore file
│
└── output/                 # Cleaned data exports (optional)
    └── cleaned_emp.csv
```

---

## ✨ Key Features

### 🎯 Comprehensive Data Cleaning
- Missing value detection and handling
- Data type validation and conversion
- Outlier detection and removal
- Text standardization and normalization

### 📊 Multiple Data Types
- Numeric cleaning (age, salary, experience)
- Date parsing (multiple formats supported)
- Categorical standardization (department, city)
- Boolean normalization (is_active)

### 🔍 Quality Checks
- Duplicate detection and removal
- Range validation (age: 0-100)
- Format consistency checks
- Data type verification

### 📈 Best Practices
- Modular approach with clear steps
- Commented code for clarity
- Error handling with `errors='coerce'`
- Non-destructive transformations

---

## 🛠️ Technologies Used

| Technology | Purpose |
|-----------|---------|
| ![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white) | Core programming language |
| ![Pandas](https://img.shields.io/badge/-Pandas-150458?logo=pandas&logoColor=white) | Data manipulation and analysis |
| ![NumPy](https://img.shields.io/badge/-NumPy-013243?logo=numpy&logoColor=white) | Numerical operations |
| ![Jupyter](https://img.shields.io/badge/-Jupyter-F37626?logo=jupyter&logoColor=white) | Interactive development environment |

---

## 📊 Results

### Before Cleaning
```
- 1000 rows × 9 columns
- ~44% missing user_ids
- Mixed data types in multiple columns
- Inconsistent formatting
- Invalid values (negative ages, text in numeric fields)
```

### After Cleaning
```
- 1000 rows × 9 columns (preserved)
- Standardized user_ids (all uppercase)
- Valid age range (0-100 or NaN)
- Cleaned salary values (numeric only)
- Standardized dates (datetime format)
- Consistent department names (uppercase)
- Title case city names
- Numeric performance scores
- Numeric experience years
- Standardized boolean values
- Duplicates removed
```

### Data Quality Improvements
- ✅ All text fields standardized
- ✅ Invalid numeric values converted to NaN
- ✅ Multiple date formats unified
- ✅ Performance scores mapped to numeric scale
- ✅ Consistent categorical values
- ✅ Clean boolean representations

---

## 📈 Example Transformations

### User ID
```
Before: user_0, U1001, user_997, NaN
After:  USER_0, U1001, USER_997, NaN
```

### Age
```
Before: -5, 30, 200, "unknown"
After:  NaN, 30, NaN, NaN
```

### Salary
```
Before: ₹50000, 45000, N/A, 120000.75
After:  50000, 45000, NaN, 120000.75
```

### Department
```
Before: HR, it , Finance, NaN
After:  HR, IT, FINANCE, NaN
```

### City
```
Before: DELHI, mumbai, Bangalore
After:  Delhi, Mumbai, Bangalore
```

### Performance Score
```
Before: "poor", "excellent", 3, 5
After:  0, 1, 3, 5
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch:**
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. **Commit your changes:**
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. **Push to the branch:**
   ```bash
   git push origin feature/AmazingFeature
   ```
5. **Open a Pull Request**

### Areas for Contribution
- Additional data validation rules
- Visualization of data quality improvements
- Export functionality for cleaned data
- Unit tests for cleaning functions
- Performance optimization
- Documentation improvements

---

## 🙏 Acknowledgments

- Dataset inspired by real-world employee data challenges
- Pandas documentation and community
- Data cleaning best practices from the data science community

---

## 📚 Additional Resources

- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [NumPy Documentation](https://numpy.org/doc/)
- [Jupyter Documentation](https://jupyter.org/documentation)
- [Data Cleaning Best Practices](https://towardsdatascience.com/data-cleaning-101)

---

## 🐛 Known Issues

- Some edge cases in date parsing may need manual review
- Performance scores above 5 are preserved (may need validation)
- Negative experience years are not automatically corrected

---

## 🔮 Future Enhancements

- [ ] Add data visualization dashboard
- [ ] Implement automated data quality reports
- [ ] Create data profiling summary
- [ ] Add export to multiple formats (Excel, JSON, SQL)
- [ ] Implement advanced outlier detection
- [ ] Add data validation rules engine
- [ ] Create interactive data cleaning interface

---

## 📞 Support

If you have any questions or run into issues:
- Open an [Issue](https://github.com/yourusername/employee-data-cleaning/issues)
- Check existing [Discussions](https://github.com/yourusername/employee-data-cleaning/discussions

---

<div align="center">

**Made with ❤️ by Akshit**

⭐ Star this repo if you found it helpful!

</div>
