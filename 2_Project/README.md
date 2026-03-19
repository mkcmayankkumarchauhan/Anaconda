# 🏨 Hotel Booking Cancellation Analysis

> An exploratory data analysis (EDA) project to identify key factors driving hotel booking cancellations and uncover actionable insights using Python.

---

## 📖 Table of Contents

- [About the Project](#about-the-project)
- [Dataset Overview](#dataset-overview)
- [Project Workflow](#project-workflow)
- [Key Findings](#key-findings)
- [Visualizations](#visualizations)
- [Technologies Used](#technologies-used)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Recommendations](#recommendations-based-on-analysis)
- [License](#license)

---

## 📌 About the Project

Hotel booking cancellations are a major challenge for the hospitality industry, leading to revenue loss and inefficient resource planning. This project performs a comprehensive **Exploratory Data Analysis (EDA)** on a real-world hotel bookings dataset to:

- Understand the overall cancellation rate
- Compare cancellation patterns between **City Hotels** and **Resort Hotels**
- Identify which **months**, **countries**, and **market segments** have the highest cancellations
- Analyze the relationship between **Average Daily Rate (ADR)** and cancellations

---

## 📊 Dataset Overview

| Property | Details |
|---|---|
| **Source** | Hotel Booking Demand Dataset |
| **Original Shape** | 119,390 rows × 36 columns |
| **Final Shape (after cleaning)** | 118,898 rows × 34 columns |
| **Date Range** | 2015 – 2017 |
| **Hotel Types** | City Hotel, Resort Hotel |

### Key Columns

| Column | Description |
|---|---|
| `hotel` | Type of hotel (City Hotel / Resort Hotel) |
| `is_canceled` | Whether the booking was cancelled (1 = Yes, 0 = No) |
| `lead_time` | Days between booking and arrival |
| `arrival_date_month` | Month of arrival |
| `adr` | Average Daily Rate (revenue per night) |
| `market_segment` | Booking channel (Online TA, Corporate, etc.) |
| `country` | Guest's country of origin |
| `deposit_type` | Type of deposit made |
| `customer_type` | Type of customer (Transient, Group, etc.) |
| `reservation_status` | Final status (Check-Out / Canceled / No-Show) |
| `total_of_special_requests` | Number of special requests made |

---

## 🔄 Project Workflow

### 1. Data Loading & Inspection
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('hotel_bookings.csv')
df.shape         # (119390, 36)
df.info()
df.describe()
```

### 2. Data Cleaning
```python
# Convert date column
df['reservation_status_date'] = pd.to_datetime(df['reservation_status_date'])

# Drop high-null columns
# company had 112,593 nulls; agent had 16,340 nulls
df.drop(['company', 'agent'], axis=1, inplace=True)

# Drop remaining null rows
df.dropna(inplace=True)

# Remove ADR outlier
df = df[df['adr'] < 5000]
```

**Null values handled:**

| Column | Action |
|---|---|
| `children` | 4 nulls — rows dropped |
| `country` | 488 nulls — rows dropped |
| `agent` | 16,340 nulls — column dropped |
| `company` | 112,593 nulls — column dropped |

### 3. Feature Engineering
```python
# Extract month name from reservation status date
df['month'] = df['reservation_status_date'].dt.month_name()
```

### 4. Exploratory Data Analysis & Visualizations

---

## 💡 Key Findings

### 🔴 Overall Cancellation Rate
- **37.1%** of all bookings were cancelled
- **62.9%** of bookings were successfully checked out

### 🏙️ Cancellation by Hotel Type
| Hotel | Cancellation Rate |
|---|---|
| City Hotel | **41.7%** |
| Resort Hotel | **28.0%** |

> City Hotels have a significantly higher cancellation rate than Resort Hotels.

### 📅 Monthly Trends
- Cancellations are **highest in the summer months** (July – August)
- **January** sees a spike in cancellation ADR

### 🌍 Top Countries by Cancellations
The top 10 countries contributing to cancellations are dominated by:

1. 🇵🇹 **PRT** (Portugal) — largest share
2. 🇬🇧 **GBR** (United Kingdom)
3. 🇫🇷 **FRA** (France)
4. 🇪🇸 **ESP** (Spain)
5. 🇩🇪 **DEU** (Germany)

*(and others — see pie chart in notebook)*

### 📣 Market Segment Insights
| Segment | Overall Share | Share in Cancellations |
|---|---|---|
| Online TA | 47.4% | 47.0% |
| Groups | 16.7% | **27.4%** |
| Offline TA/TO | 20.3% | 18.7% |
| Direct | 10.5% | 4.3% |

> **Groups** are disproportionately responsible for cancellations relative to their overall booking share.

### 💰 ADR vs Cancellations
- Cancelled bookings tend to have a **higher ADR** compared to non-cancelled ones
- This trend is consistent across **2016–2017**, suggesting price sensitivity plays a role in cancellations

---

## 📈 Visualizations

| # | Chart Type | Description |
|---|---|---|
| 1 | Bar Chart | Overall reservation status (Cancelled vs Not Cancelled) |
| 2 | Count Plot | Cancellations by hotel type |
| 3 | Line Plot | Average Daily Rate over time — City vs Resort Hotel |
| 4 | Count Plot | Monthly cancellation distribution |
| 5 | Bar Plot | Average ADR per month for cancelled bookings |
| 6 | Pie Chart | Top 10 countries with highest cancellations |
| 7 | Line Plot | ADR comparison — Cancelled vs Not Cancelled (2016–2017) |

---

## 🛠️ Technologies Used

| Tool | Purpose |
|---|---|
| **Python 3.10+** | Core programming language |
| **Pandas** | Data loading, cleaning, and manipulation |
| **Matplotlib** | Base plotting |
| **Seaborn** | Statistical visualizations |
| **Jupyter Notebook** | Interactive analysis environment |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10 or higher
- Jupyter Notebook or JupyterLab

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/your-username/hotel-booking-analysis.git
cd hotel-booking-analysis
```

2. **Create and activate a virtual environment**
```bash
python -m venv venv

# macOS/Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Launch Jupyter Notebook**
```bash
jupyter notebook
```

5. **Open and run the notebook**

Navigate to `hotel_booking_analysis.ipynb` and run all cells in order.

### `requirements.txt`
```
pandas
matplotlib
seaborn
jupyter
```

---

## 📁 Project Structure

```
hotel-booking-analysis/
│
├── hotel_bookings.csv                  # Raw dataset
├── hotel_booking_analysis.ipynb        # Main Jupyter Notebook
│
├── plots/                              # Saved visualizations
│   ├── reservation_status.png
│   ├── cancellation_by_hotel.png
│   ├── adr_over_time.png
│   ├── monthly_cancellations.png
│   ├── adr_per_month.png
│   ├── top10_countries_pie.png
│   └── adr_cancelled_vs_not.png
│
├── requirements.txt
└── README.md
```

---

## 📋 Recommendations (Based on Analysis)

- 🏙️ **City Hotels** should implement stricter cancellation policies or non-refundable deposit options to reduce their 41.7% cancellation rate.
- 🌐 **Online TA & Group bookings** need targeted policies — these segments drive the most cancellations.
- 💵 **High ADR periods** (peak season) correlate with more cancellations — consider dynamic pricing strategies or flexible booking incentives to retain customers.
- 🌍 Focus retention strategies on guests from **Portugal, UK, France, and Spain**, who represent the bulk of cancellations.
- 📅 Offer **early-bird discounts** during high-cancellation months (July–August) to lock in confirmed bookings.






---

<p align="center">Made with ❤️ using Python & Jupyter Notebook</p>
