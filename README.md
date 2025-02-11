# AWS Support Engineering Utility for PostgreSQL

## Overview
The AWS Support Engineering (SE) Utility is a PostgreSQL extension that provides tools for checking database readiness for AWS Database Migration Service (DMS) and version upgrades. Built using pg_tle (Trusted Language Extensions), this utility offers a collection of functions to validate configurations and identify potential issues.

## Installation

### Prerequisites
* PostgreSQL database running on Amazon RDS or Aurora PostgreSQL
* pg_tle extension installed
* Appropriate permissions to create extensions

### Installation Steps
```sql
-- 1. Install pg_tle if not already installed
CREATE EXTENSION IF NOT EXISTS pg_tle;

-- 2. Install the AWS SE Utility extension
CREATE EXTENSION aws_se_utility;
```

## Features

### DMS Readiness Check
The DMS readiness check validates if your PostgreSQL database meets the requirements for AWS DMS.

```sql
-- View DMS readiness status
SELECT * FROM aws_se_utility.dms_readiness_check;
```

#### Checks Performed:
* User permissions (rds_superuser and rds_replication roles)
* Logical replication configuration
* WAL sender timeout settings
* Worker processes configuration
* Synchronous commit settings
* WAL level configuration
* Replication slots
* WAL senders configuration

### Upgrade Readiness Check
The upgrade readiness check helps identify potential issues before upgrading your PostgreSQL version.

```sql
-- View all upgrade readiness checks
SELECT * FROM aws_se_utility.upgrade_readiness_check;

-- View only issues that need attention
SELECT * FROM aws_se_utility.get_upgrade_issues();
```

#### Checks Performed:
* Unknown data type usage
* Open prepared transactions
* Unsupported reg* types
* Template database configuration
* Logical replication slots
* Parameter group status
* pgRouting extension compatibility
* pg_repack extension compatibility

## Function Reference

### check_dms_readiness()
Returns a table of DMS prerequisite checks with their status.

```sql
SELECT * FROM aws_se_utility.check_dms_readiness();
```

**Returns:**
* `check_name`: Name of the check performed
* `status`: OK, WARNING, or ERROR
* `details`: Detailed description of the check result

### check_upgrade_readiness()
Performs comprehensive checks for database upgrade compatibility.

```sql
SELECT * FROM aws_se_utility.check_upgrade_readiness();
```

### Status Codes
* **OK**: Check passed successfully
* **WARNING**: Potential issue that should be reviewed
* **ERROR**: Critical issue that must be resolved

## Best Practices
1. Run checks during maintenance windows
2. Address all ERROR status items before proceeding
3. Review WARNING status items and assess their impact
4. Maintain proper backups before making changes
5. Test changes in non-production environments first

## Troubleshooting

### Common Issues
**Permission Errors:**
```sql
-- Grant necessary permissions
GRANT USAGE ON SCHEMA aws_se_utility TO <user>;
GRANT EXECUTE ON ALL FUNCTIONS IN SCHEMA aws_se_utility TO <user>;
```

**Extension Installation Issues:**
```sql
-- Reinstall the extension
DROP EXTENSION IF EXISTS aws_se_utility CASCADE;
CREATE EXTENSION aws_se_utility;
```

## Version History
### Version 1.0
* Initial release
* DMS readiness checks
* Upgrade readiness checks
* Formatted issue reporting

## Support
For issues and feature requests, please contact AWS Support or your account team.
