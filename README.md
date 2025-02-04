# MandrakeTech
Test Case Writing, Automation, and Bug Report for a Registration Form
1. Test Cases
Objective: Validate functionality, security, and edge-case handling for a registration form.
ID	Priority	Test Scenario	Steps	Expected Result	Status
TC-001	High	Valid registration	Fill all fields correctly and submit.	Success message: 'Registration Complete'.	Pass
TC-002	High	Invalid email format (user@domain)	Enter invalid email and submit.	Error: 'Invalid email format'.	Fail
TC-003	Critical	Empty first name field	Leave first name blank and submit.	Error: 'First name is required'.	Pass
TC-004	High	Password mismatch	Enter mismatched passwords and submit.	Error: 'Passwords do not match'.	Pass
TC-005	Medium	Weak password (no special character)	Enter Password123 and submit.	Error: 'Include 1 special character'.	Fail
TC-006	High	Non-numeric phone number (123-456-789)	Enter invalid phone and submit.	Error: 'Phone must be numeric'.	Fail
TC-007	Medium	Duplicate email	Use an existing email and submit.	Error: 'Email already registered'.	Pass
2. Automation Script
Objective: Automate TC-005 (Password Complexity Validation) using Python, Selenium, and pytest.
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

@pytest.fixture(scope="module")
def setup():
    driver = webdriver.Chrome()
    driver.get("https://example.com/register")
    yield driver
    driver.quit()

def test_password_complexity(setup):
    driver = setup
    password_field = driver.find_element(By.ID, "password")
    submit_btn = driver.find_element(By.ID, "submit-btn")
    
    # Test weak password
    password_field.send_keys("Password123")
    submit_btn.click()
    
    # Verify error message
    error_msg = WebDriverWait(driver, 10).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, ".error-message"))
    )
    assert "special character" in error_msg.text.lower(), "Password validation failed"

3. Bug Report
Title: Phone Number Field Accepts Non-Numeric Characters
Severity: High | Priority: Urgent
Description	Phone field allows letters/symbols (e.g., abc, 123-456), causing invalid registrations.
Steps to Reproduce	1. Go to registration page.
2. Enter abcde in phone field.
3. Submit.
Expected Result	Error: 'Phone number must be 10 numeric digits'.
Actual Result	Form submits with invalid phone number.
Root Cause	Missing regex validation (`^[0-9]{10}$`).
Impact	Database corruption, failed SMS notifications.
4. Additional Deliverables
A. Regression Test Suite
- [x] TC-001: Valid Registration
- [ ] TC-005: Password Complexity
- [ ] TC-006: Phone Number Validation
B. Test Summary Report
Coverage: 100% of critical fields (email, password, phone).
Defects Found: 3 (Email format, password rules, phone validation).
Recommendations:
- Add CAPTCHA to prevent bot registrations.
- Implement input masking for phone numbers.
5. Conclusion
This assignment demonstrates industry-standard testing practices, including:
- Comprehensive test design (boundary values, edge cases).
- Robust automation with Python, Selenium, and pytest.
- Professional bug tracking with root cause analysis.
