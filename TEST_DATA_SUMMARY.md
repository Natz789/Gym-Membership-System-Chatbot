# Test Data Setup - Comprehensive Database Seeding

## Summary

Test data has been successfully created and populated in the database. This provides a comprehensive testing environment for the Gym Management System.

### Date Created: November 22, 2025

## Seeded Test Data Overview

### 👥 Users (58 Total)
- **Admins**: 3 accounts
  - Username: `admin` | Password: `admin123`
  - Username: `manager` | Password: `admin123`
  - Username: `director` | Password: `admin123`

- **Staff**: 5 accounts
  - Usernames: `staff1` - `staff5` | Password: `staff123`
  - All have realistic names and emails (Philippine-based)

- **Members**: 50 accounts
  - Password: `member123`
  - All have auto-generated 6-digit kiosk PINs
  - Realistic Filipino names and addresses
  - Age range: 18-65 years old
  - Mobile numbers in Philippine format (09XXXXXXXXX)

### 💳 Membership Plans (7 Total)

#### Active Plans (5):
1. **Weekly Pass** - ₱500 (7 days)
2. **Monthly Membership** - ₱1,500 (30 days)
3. **Quarterly Membership** - ₱4,000 (90 days)
4. **Semi-Annual Premium** - ₱7,500 (180 days)
5. **Annual VIP Membership** - ₱14,000 (365 days)

#### Archived Plans (2):
- Student Special
- Senior Citizen Plan

### 🎫 Walk-in Passes (4 Total)

#### Active Passes (3):
1. **Single Day Pass** - ₱100 (1 day)
2. **3-Day Trial Pass** - ₱250 (3 days)
3. **5-Day Flex Pass** - ₱400 (5 days)

#### Archived Passes (1):
- Weekend Warrior

### 📋 User Memberships (68 Total)
- **Active**: 40 (ongoing subscriptions)
- **Expired**: 23 (for testing expiration logic)
- **Pending**: 4 (awaiting payment confirmation)
- **Cancelled**: 1 (for testing cancellation workflow)

### 💰 Member Payments (67 Total)
- **Confirmed**: 52 (✓ Activated memberships)
- **Pending**: 11 (awaiting staff/admin approval)
- **Rejected**: 4 (with rejection reasons)
- **Total Revenue**: ₱270,500

### 🚶 Walk-in Sales (568 Total)
- **Cash Payments**: 288
- **GCash Payments**: 280
- **Total Revenue**: ₱136,600
- **Date Range**: Last 90 days (2-8 per day average)

### 📊 Attendance Records (1,607 Total)
- **Completed Sessions**: 1,606 (with check-in and check-out)
- **Currently Checked In**: 1
- **Realistic Check-in Times**: 6 AM - 9 PM
- **Realistic Duration**: 30 minutes to 3 hours per session
- **Coverage**: 90 days of historical data

### 🔐 Login Activity (1,498 Total)
- **Successful Logins**: 1,404
- **Failed Logins**: 94
- **Realistic Login Patterns**: Throughout 90-day period

### 🤖 Chatbot Integration (38 Conversations)
- **Total Messages**: 76
- **Active Model**: Llama 3.2 1B (optimized for 8GB RAM)
- **Conversation History**: Various member and staff interactions

### 📈 Analytics (90 Records)
- **Daily Aggregated Data**: 90 days of statistics
- **Metrics Include**: Members, passes sold, revenue, age groups
- **Time Period**: Last 90 days

### 📝 Audit Logs (500+ Entries)
- **User Actions Tracked**: Login, logout, registration, updates
- **Role Changes**: Tracked with timestamps
- **Membership Events**: Creation, updates
- **Data Integrity**: All changes logged with IP addresses

## Total Financial Summary
**Combined Revenue**: ₱406,600
- Member Payments: ₱270,500
- Walk-in Payments: ₱136,600

## System Features Demonstrated

✓ **User Management** - Multi-role access control (Admin, Staff, Member)
✓ **Authentication** - Login/logout tracking with failed attempt logging
✓ **Membership System** - Full lifecycle (pending → active → expired/cancelled)
✓ **Payment Processing** - Multiple statuses (pending, confirmed, rejected)
✓ **Walk-in System** - Unregistered customer sales with reference tracking
✓ **Attendance Tracking** - Check-in/check-out with duration calculation
✓ **Kiosk System** - 6-digit PIN verification (all members have PINs)
✓ **Payment Methods** - Cash and GCash support
✓ **AI Chatbot** - Ollama integration with conversation persistence
✓ **Analytics** - Daily reporting and trend analysis
✓ **Audit Trail** - Comprehensive logging of all system activities

## How to Use Test Data

### Login with Different Roles

```
Admin:   username=admin,   password=admin123
Staff:   username=staff1,  password=staff123
Member:  username=<any member name>, password=member123
```

### Test Scenarios

1. **Approval Workflow** - Log in as admin/staff and approve pending payments
2. **Membership Status** - View active, expired, and cancelled memberships
3. **Attendance Analytics** - Check attendance patterns and duration statistics
4. **Revenue Reports** - View combined member and walk-in revenue
5. **Chatbot Testing** - Test AI responses with chatbot widget
6. **Audit Trail** - Review system activities in audit logs
7. **Kiosk System** - Test 6-digit PIN check-in/out (use member PIN)

### Database Information

- **Database Type**: SQLite3
- **Location**: `db.sqlite3` (in project root)
- **Size**: ~1.2 MB with all test data
- **Migrations**: All applied and current

## Features Ready for Testing

✅ Dashboard views and statistics
✅ Payment approval workflows
✅ Membership lifecycle management
✅ Attendance tracking and reports
✅ Chatbot responses and conversations
✅ Audit trail and activity logs
✅ Walk-in transaction processing
✅ Multi-payment method support (cash/gcash)
✅ Member account management
✅ Staff and admin operations

## Refreshing Test Data

To reseed the database with fresh test data at any time:

```bash
# Run with 90 days of data and 50 members (default)
python manage.py comprehensive_seeder --flush --days 90

# Or customize parameters
python manage.py comprehensive_seeder --flush --users 100 --days 180
```

## Database Cleanup

If you need to completely reset the database:

```bash
# Delete and recreate migrations
python manage.py migrate gym_app zero
python manage.py migrate
python manage.py comprehensive_seeder --flush
```

---

**Status**: ✅ Test data successfully populated and verified
**Data Integrity**: All relationships and constraints verified
**Ready for**: System testing, feature development, UAT
