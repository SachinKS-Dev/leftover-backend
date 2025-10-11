# PDF Financial Reports Guide

## Overview

The financial reports system now supports PDF generation and download functionality. You can generate PDF reports for daily, weekly, monthly, and custom date range reports.

## Features Added

### 1. PDF Generation

- Professional PDF reports with tables and formatting
- Color-coded sections for easy readability
- Comprehensive financial data including:
  - Summary statistics
  - Payment method breakdown
  - Delivery status breakdown
  - Daily breakdown (for date ranges)
  - Top delivery boys performance
  - COD information

### 2. Download Options

Each report type now has a "Download PDF Report" button that allows you to:

- Download the report as a PDF file
- Save it to your local system
- Share it with stakeholders
- Print it for physical records

## How to Use

### Accessing Financial Reports

1. Log in to the Django admin panel at: `http://127.0.0.1:8000/admin/`
2. Navigate to: **Food Management** > **Order details**
3. Click on the **Financial Reports** link in the top right

### Generating Reports

#### Daily Report

1. From the Financial Reports Dashboard, select a date
2. Click "Generate Daily Report"
3. On the report page, click the "📄 Download PDF Report" button
4. The PDF will be downloaded with filename format: `daily_report_YYYYMMDD.pdf`

#### Weekly Report

1. From the Financial Reports Dashboard, select a week
2. Click "Generate Weekly Report"
3. On the report page, click the "📄 Download PDF Report" button
4. The PDF will be downloaded with filename format: `weekly_report_YYYYMMDD_YYYYMMDD.pdf`

#### Monthly Report

1. From the Financial Reports Dashboard, select a month
2. Click "Generate Monthly Report"
3. On the report page, click the "📄 Download PDF Report" button
4. The PDF will be downloaded with filename format: `monthly_report_YYYYMM.pdf`

#### Custom Date Range Report

1. From the Financial Reports Dashboard, select a start and end date
2. Click "Generate Custom Report"
3. On the report page, click the "📄 Download PDF Report" button
4. The PDF will be downloaded with filename format: `custom_report_YYYYMMDD_YYYYMMDD.pdf`

## PDF Report Contents

Each PDF report includes:

### Summary Section

- Total Orders
- Total Revenue
- Platform Fee
- Delivery Charges
- COD Collected
- Average Order Value

### Payment Method Breakdown

- Orders by payment method (Wallet, Online, COD)
- Amount and percentage for each method

### Delivery Status Breakdown

- Orders by delivery status
- Amount and percentage for each status

### Daily Breakdown (if applicable)

- Date-wise breakdown of:
  - Orders count
  - Revenue
  - Platform fee
  - Delivery charges
  - COD collected

### Top Delivery Boys (if applicable)

- Delivery boy performance
- Total earnings
- Number of deliveries
- Average earning per delivery

### COD Information

- COD Collected
- COD Submitted
- Pending COD

## Technical Details

### Dependencies

- `reportlab` (v4.0.0+) - For PDF generation
- `pillow` - For image handling (dependency of reportlab)

### Files Modified/Added

1. **New Files:**

   - `core/pdf_generator.py` - PDF generation utility

2. **Modified Files:**
   - `foodmanagement/admin.py` - Added PDF download endpoints
   - `foodmanagement/templates/admin/foodmanagement/orderdetails/report_detail.html` - Added download button
   - `pyproject.toml` - Added reportlab dependency

### URL Endpoints

- Daily PDF: `/admin/foodmanagement/orderdetails/financial-reports/daily/pdf/?date=YYYY-MM-DD`
- Weekly PDF: `/admin/foodmanagement/orderdetails/financial-reports/weekly/pdf/?week=YYYY-W##`
- Monthly PDF: `/admin/foodmanagement/orderdetails/financial-reports/monthly/pdf/?month=YYYY-MM`
- Custom PDF: `/admin/foodmanagement/orderdetails/financial-reports/custom/pdf/?start_date=YYYY-MM-DD&end_date=YYYY-MM-DD`

## Troubleshooting

### PDF Not Generating

1. Ensure ReportLab is installed: `poetry install`
2. Check that the virtual environment is activated
3. Verify that there's data in the selected date range

### Styling Issues

The PDF uses the ReportLab library for styling. If you need to customize:

- Edit `core/pdf_generator.py`
- Modify the color schemes (currently using green theme: #2E7D32)
- Adjust table layouts and font sizes

### Performance

- Large date ranges may take longer to generate
- Consider generating reports for shorter periods if performance is an issue

## Customization

To customize the PDF appearance, edit `core/pdf_generator.py`:

- Change colors by modifying `colors.HexColor('#2E7D32')`
- Adjust table widths by changing the `colWidths` parameter
- Modify fonts and sizes in the `ParagraphStyle` definitions

## Support

For any issues or questions, refer to:

- ReportLab documentation: https://www.reportlab.com/docs/reportlab-userguide.pdf
- Django admin documentation: https://docs.djangoproject.com/en/stable/ref/contrib/admin/
