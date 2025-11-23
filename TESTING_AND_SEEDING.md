# Testing & Seeding Guide for GymFit Pro

Comprehensive guide for seeding test data and running tests for GCash QR code functionality and payment processing.

**Document Version**: 1.0
**Last Updated**: November 22, 2025
**Status**: ✅ Complete and Tested

---

## Quick Start

### Seed Test Data
```bash
# Create 10 members with GCash payments and 20 walk-in transactions
python manage.py gcash_test_seeder --members 10 --walkin 20

# Run with flush to reset data first
python manage.py gcash_test_seeder --flush --members 10 --walkin 20
```

### Run Tests
```bash
# Run all tests
python manage.py test gym_app.tests

# Run specific test class
python manage.py test gym_app.tests.GCashQRCodeGenerationTest

# Run with verbose output
python manage.py test gym_app.tests -v 2
```

---

## Seeding Overview

### Purpose
Create realistic test data specifically for GCash payment QR code functionality testing and validation.

### Features

**GCash-Focused Data:**
- Members with GCash subscription payments
- Walk-in customers with GCash pass purchases
- Mix of pending and confirmed payments (for testing both states)
- Test plans and passes at various price points
- Realistic reference numbers and payment data

**Test Data Distribution:**
- ~60% confirmed payments (with QR codes already generated)
- ~40% pending payments (ready for manual confirmation testing)
- Various membership plan prices (₱500 - ₱14,000)
- Various pass prices (₱100 - ₱400)

---

## Seeder Command: `gcash_test_seeder`

### Location
`gym_app/management/commands/gcash_test_seeder.py`

### Usage

#### Basic Usage
```bash
python manage.py gcash_test_seeder
```
Default: 10 members, 20 walk-in payments

#### Custom Configuration

**Create more members with their payment data:**
```bash
python manage.py gcash_test_seeder --members 25
```

**Create more walk-in transactions:**
```bash
python manage.py gcash_test_seeder --walkin 50
```

**Both combined:**
```bash
python manage.py gcash_test_seeder --members 15 --walkin 30
```

**Flush existing test data before seeding:**
```bash
python manage.py gcash_test_seeder --flush
```

**Full example with all options:**
```bash
python manage.py gcash_test_seeder --flush --members 20 --walkin 40
```

### What Gets Created

#### Users
- Test staff member: `staff_test` / `staff123` (role: staff)
- Test members: `gcash_member_1` through `gcash_member_N` (role: member)
  - Password for all members: `member123`
  - Each has unique email and mobile number

#### Plans (All for GCash testing)
1. **GCash Test - Weekly** - ₱500 (7 days)
2. **GCash Test - Monthly** - ₱1,500 (30 days)
3. **GCash Test - Quarterly** - ₱4,000 (90 days)

#### Passes (All for GCash testing)
1. **GCash Test - Day Pass** - ₱100 (1 day)
2. **GCash Test - 3-Day Trial** - ₱250 (3 days)
3. **GCash Test - 5-Day Flex** - ₱400 (5 days)

#### Memberships & Payments
- One membership subscription per test member
- Each membership has corresponding GCash payment
- Payment status distributed: ~60% confirmed, ~40% pending
- All payments use `method='gcash'`
- Auto-generated reference numbers: `PAY-YYYYMMDD-XXXXXX`

#### Walk-in Payments
- GCash payments for walk-in customers
- Optional customer name and mobile number (70% have names, 50% have phone)
- Auto-generated reference numbers: `WLK-YYYYMMDD-XXXXXX`
- All payments use `method='gcash'`

### Example Output

```
================================================================================
🤖 GCASH TEST DATA SEEDER
================================================================================
Configuration: 10 Members | 20 Walk-ins
Focused on GCash payments for QR code testing
================================================================================

👥 Ensuring Admin/Staff Users...
   ✓ Created test staff user

💳 Ensuring Membership Plans...
   ✓ Created: GCash Test - Weekly
   ✓ Created: GCash Test - Monthly
   ✓ Created: GCash Test - Quarterly

🎫 Ensuring Flexible Access Passes...
   ✓ Created: GCash Test - Day Pass
   ✓ Created: GCash Test - 3-Day Trial
   ✓ Created: GCash Test - 5-Day Flex

👤 Creating 10 Members with GCash Memberships...
   ✓ Created 10 new members
   ℹ Confirmed payments: 6
   ℹ Pending payments: 4 (for manual testing)

🚶 Creating 20 GCash Walk-in Payments...
   ✓ Created 20 GCash walk-in payments

================================================================================
✅ GCASH TEST DATA SEEDING COMPLETE!
================================================================================

👥 MEMBERS
   Test Members: 10

💳 MEMBERSHIPS (GCash)
   Total: 10
   Active: 6
   Pending: 4

💰 MEMBER PAYMENTS (GCash)
   Total: 10
   Confirmed: 6 (with generated QR codes)
   Pending: 4 (ready for confirmation testing)
   Revenue: ₱21,500.00

🚶 WALKIN PAYMENTS (GCash)
   Total: 20
   Revenue: ₱7,450.00

================================================================================
🧪 READY FOR QR CODE TESTING
================================================================================

📋 TEST SCENARIOS:
  1. Test pending payment confirmation QR code (view pending-payments)
  2. Test confirmed payment display
  3. Test walk-in payment QR code generation
  4. Verify QR code data encoding
  5. Test fallback to static image if needed

🔑 TEST CREDENTIALS:
  Admin: admin / admin123
  Staff: staff_test / staff123
  Members: gcash_member_1 / member123 (and more)

================================================================================
```

---

## Testing Overview

### Test Framework
- Django's built-in `TestCase` framework
- 12 comprehensive test cases covering QR code and payment functionality
- All tests pass successfully ✅

### Test File Location
`gym_app/tests.py`

### Test Classes

#### 1. GCashQRCodeGenerationTest (3 tests)
Tests dynamic QR code generation functionality

**Tests:**
- `test_qr_code_generation_success` - Verify QR code generates without errors
- `test_qr_code_with_different_amounts` - Test various payment amounts
- `test_merchant_info_retrieval` - Verify merchant info is accessible

**Example:**
```bash
python manage.py test gym_app.tests.GCashQRCodeGenerationTest
```

#### 2. PaymentModelTest (4 tests)
Tests Payment model and confirmation/rejection logic

**Tests:**
- `test_payment_creation_gcash` - Create GCash payment record
- `test_payment_reference_unique` - Verify unique reference numbers
- `test_payment_confirmation` - Confirm payment and check status
- `test_payment_rejection` - Reject payment with reason

**Example:**
```bash
python manage.py test gym_app.tests.PaymentModelTest
```

#### 3. WalkInPaymentTest (3 tests)
Tests WalkInPayment model and reference generation

**Tests:**
- `test_walkin_payment_creation_gcash` - Create walk-in GCash payment
- `test_walkin_payment_without_customer_info` - Anonymous walk-in payment
- `test_walkin_payment_reference_format` - Validate reference number format

**Example:**
```bash
python manage.py test gym_app.tests.WalkInPaymentTest
```

#### 4. IntegrationTest (2 tests)
End-to-end tests for complete payment flows

**Tests:**
- `test_complete_gcash_membership_flow` - Full member subscription → payment → confirmation
- `test_complete_gcash_walkin_flow` - Complete walk-in GCash transaction

**Example:**
```bash
python manage.py test gym_app.tests.IntegrationTest
```

---

## Running Tests

### Run All Tests
```bash
python manage.py test gym_app.tests
```

### Run Specific Test Class
```bash
python manage.py test gym_app.tests.GCashQRCodeGenerationTest
python manage.py test gym_app.tests.PaymentModelTest
python manage.py test gym_app.tests.WalkInPaymentTest
python manage.py test gym_app.tests.IntegrationTest
```

### Run Specific Test Method
```bash
python manage.py test gym_app.tests.GCashQRCodeGenerationTest.test_qr_code_generation_success
```

### Verbose Output
```bash
python manage.py test gym_app.tests -v 2
```

### Keep Test Database for Inspection
```bash
python manage.py test gym_app.tests --keepdb
```

### Quiet Output (Minimal)
```bash
python manage.py test gym_app.tests -v 0
```

---

## Test Results

### Current Status: ✅ All Tests Pass

```
Ran 12 tests in 4.347s

OK
```

### Test Breakdown

| Test Class | Tests | Status |
|-----------|-------|--------|
| GCashQRCodeGenerationTest | 3 | ✅ Pass |
| PaymentModelTest | 4 | ✅ Pass |
| WalkInPaymentTest | 3 | ✅ Pass |
| IntegrationTest | 2 | ✅ Pass |
| **TOTAL** | **12** | **✅ Pass** |

---

## Testing Workflows

### Workflow 1: Test Membership Payment Confirmation

**Setup:**
```bash
python manage.py gcash_test_seeder --flush --members 5 --walkin 0
```

**Steps:**
1. Log in with staff credentials: `staff_test` / `staff123`
2. Navigate to `/pending-payments/`
3. Find a GCash payment marked as "pending"
4. Click "Confirm"
5. Verify:
   - Dynamic QR code displays
   - Merchant info shows
   - Payment instructions appear
   - Reference number is correct
6. Click "Confirm Payment"
7. Verify payment is confirmed

**Expected Results:**
- ✅ QR code is generated with payment-specific data
- ✅ Page doesn't crash if QR generation fails
- ✅ All merchant info displays correctly
- ✅ Payment status changes to "confirmed"
- ✅ Membership becomes "active"

### Workflow 2: Test Walk-in Payment QR Code

**Setup:**
```bash
python manage.py gcash_test_seeder --members 0 --walkin 10
```

**Steps:**
1. Log in with staff credentials
2. Navigate to `/walkin/`
3. Select a pass and GCash payment method
4. Click next to confirmation page
5. Verify:
   - Dynamic QR code displays
   - Correct payment amount shown
   - Reference number matches
   - Merchant details are correct
6. Click "Confirm Payment"

**Expected Results:**
- ✅ QR code shows for walk-in payment
- ✅ Amount and reference are visible
- ✅ Walk-in payment is recorded
- ✅ Revenue is calculated correctly

### Workflow 3: Verify QR Code Data

**Using Django Shell:**
```bash
python manage.py shell
```

**Commands:**
```python
from gym_app.models import Payment
from gym_app.utils import generate_gcash_qr_code

# Get a GCash payment
payment = Payment.objects.filter(method='gcash', status='pending').first()

# Generate QR code
qr_code = generate_gcash_qr_code(payment.amount, payment.reference_no)

# Verify it's valid
print(qr_code[:50])  # Should start with: data:image/png;base64,
print(len(qr_code))  # Should be > 500 characters
```

### Workflow 4: Test Data Integrity

**Verify Data Consistency:**
```python
from gym_app.models import Payment, WalkInPayment

# Check all GCash payments have references
gcash_payments = Payment.objects.filter(method='gcash')
print(f"Total GCash payments: {gcash_payments.count()}")
print(f"With references: {gcash_payments.exclude(reference_no__isnull=True).count()}")

# Check walk-in payment format
walkins = WalkInPayment.objects.filter(method='gcash')
for w in walkins[:5]:
    parts = w.reference_no.split('-')
    assert parts[0] == 'WLK', "Invalid reference format"
    print(f"✓ {w.reference_no}")
```

---

## Manual Testing Scenarios

### Scenario 1: GCash Membership Payment
**Goal:** Verify complete membership purchase with GCash and QR code

1. **Prepare:**
   - Seed data: `python manage.py gcash_test_seeder --members 5`
   - Log in as staff

2. **Test:**
   - Go to pending payments
   - Find a GCash membership payment
   - Click "Confirm Payment"
   - Take screenshot of QR code display
   - Verify all fields match:
     - Amount
     - Member name
     - Plan name
     - Reference number
   - Confirm payment

3. **Verify:**
   - Payment status → confirmed
   - Membership status → active
   - Kiosk PIN → generated
   - Audit log → payment_received entry

### Scenario 2: Walk-in GCash Pass
**Goal:** Verify walk-in QR code generation and transaction recording

1. **Prepare:**
   - Seed data: `python manage.py gcash_test_seeder --walkin 20`
   - Log in as staff

2. **Test:**
   - Go to walk-in purchase
   - Fill in customer info (optional)
   - Select a pass
   - Choose GCash payment
   - Click next
   - Verify QR code displays

3. **Verify:**
   - QR code shows correct amount
   - Reference number is present
   - Customer info (if entered) shows
   - Confirm transaction
   - Walk-in payment is recorded

### Scenario 3: Cash vs GCash
**Goal:** Verify that QR code only shows for GCash

1. **Test:**
   - Create a cash payment
   - View confirmation page
   - Verify NO QR code section
   - Create GCash payment
   - View confirmation page
   - Verify QR code appears

### Scenario 4: Fallback Behavior
**Goal:** Verify graceful fallback if QR generation fails

1. **Test:**
   - Temporarily disable QR generation in view (set qr_code_data = None)
   - Load payment confirmation page
   - Verify page still displays all information
   - Verify static image shows as fallback (if configured)

---

## Troubleshooting

### Tests Fail with Import Errors
**Solution:**
```bash
# Reinstall dependencies
pip install qrcode[pil]

# Run migrations
python manage.py migrate
```

### Seeder Creates No Data
**Solution:**
```bash
# Check Django migrations
python manage.py migrate --list

# Apply all migrations
python manage.py migrate
```

### QR Code Not Generating
**Solution:**
```bash
# Test in shell
python manage.py shell
from gym_app.utils import generate_gcash_qr_code
qr = generate_gcash_qr_code(1500, "PAY-TEST")
print(qr)  # Should output data URI, not None
```

### Payment Tests Fail
**Solution:**
```bash
# Check if test database is clean
python manage.py test gym_app.tests --keepdb

# Then manually inspect
python manage.py shell
from gym_app.models import Payment
print(Payment.objects.count())
```

---

## Test Data Cleanup

### Delete Test Seeded Data Only
```python
# In Django shell
from gym_app.models import User, Payment, WalkInPayment, UserMembership

# Delete test members
User.objects.filter(username__startswith='gcash_member').delete()

# Delete test payments
Payment.objects.filter(notes__contains='Test GCash').delete()

# Delete test walk-ins
WalkInPayment.objects.filter(
    created_at__gte='2025-11-22'  # Adjust date as needed
).delete()
```

### Reset Everything
```bash
# WARNING: This deletes ALL payments and memberships
python manage.py gcash_test_seeder --flush

# Or manually
python manage.py flush
python manage.py migrate
```

---

## Documentation References

### Related Docs
- `GCASH_QR_CODE_IMPLEMENTATION.md` - QR code feature details
- `TEST_DATA_SUMMARY.md` - Overall test data overview
- `SEEDER_README.md` - Comprehensive seeder documentation

### Code References
- Seeder: `gym_app/management/commands/gcash_test_seeder.py`
- Tests: `gym_app/tests.py`
- Utils: `gym_app/utils.py`
- Views: `gym_app/views.py` (confirm_payment, walkin_confirm)
- Models: `gym_app/models.py` (Payment, WalkInPayment)

---

## CI/CD Integration

### Run Tests in CI Pipeline
```bash
# Run all tests
python manage.py test gym_app.tests

# Run with coverage
coverage run --source='gym_app' manage.py test gym_app.tests
coverage report
coverage html
```

### GitHub Actions Example
```yaml
name: Run Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.11
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Run tests
        run: python manage.py test gym_app.tests
```

---

## Best Practices

### When Adding New Features

1. **Create test cases first**
   ```python
   class YourFeatureTest(TestCase):
       def test_your_feature(self):
           # Test code
   ```

2. **Update seeder if needed**
   - Add methods to `gcash_test_seeder.py`
   - Generate relevant test data

3. **Run tests before committing**
   ```bash
   python manage.py test gym_app.tests -v 2
   ```

4. **Document test scenarios**
   - Add to TESTING_AND_SEEDING.md
   - Explain expected behavior

### Test Data Isolation

- Each test class has `setUp()` method
- Test data is automatically cleaned up
- No data persists between test runs
- Safe to run tests multiple times

### Performance Considerations

- Keep seeded data reasonable (10-50 items for quick testing)
- Use `--users` and `--days` flags to control data volume
- Tests run in memory (SQLite database)
- ~4-5 seconds for full test suite

---

## Summary

| Component | Status | Details |
|-----------|--------|---------|
| **Seeder** | ✅ Working | `gcash_test_seeder` command ready |
| **Tests** | ✅ All Pass | 12 tests covering QR code & payments |
| **Documentation** | ✅ Complete | This guide + implementation docs |
| **Test Data** | ✅ Available | Members, payments, walks-in |
| **QR Generation** | ✅ Functional | Dynamic QR codes with fallback |
| **Integration** | ✅ Tested | End-to-end payment flows verified |

---

**Version**: 1.0
**Status**: ✅ Production Ready
**Last Tested**: November 22, 2025
**Test Results**: 12/12 Passed
