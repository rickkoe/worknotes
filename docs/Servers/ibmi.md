# IBMi
## IBMi Migration Cleanup Steps
### Remove failed and non-reporting resources
- Activate partition in B/manual
- Option 3  for DST main menu
- Log in
- Option 7 Start a service tool
- Option 4 hardware service manager
- Option 4  failed and non-reporting resources
- Delete all failed and non-reporting resources with a 4 on each line (enter)

*This should remove all logical names for the internal disks so they are not part of the configuration and prevent errors*
### Call multipathresetter
From DST main menu:
- Option 7 Start a service tool
- Option 1 Display/Alter/Dump
- Option 1 Display/Alter Storage
- Option  2 License Internal Code (LIC) data
- Option 14 Advance Analysis
- 3 page downs, "1" on MULTIPATHRESETTER (enter)
- Use options "-resetmp -all" (enter)
- Use options "-confirm -all" (enter)

*Results will show and success at the end*

### Check paths and number of Units
From DST main menu:
- Option 4 work with disk units
- Option 1 work with disk configuration
- Option 1 display disk configuration
- Option 9 display disk path status

*Each unit should have required number of active and passive path (typically 2/2 or 4/4).  IBMi supports a maximum of 8 paths.  Do not zone more than 8 paths.*

### Restricted IPL
From DST main menu:
- Option 1 Perform an IPL (enter) went to a screen showing C6004065)
- If Prompted due to non-configured disks that are known choose Option 1 to keep configuration
- Log In
- IPL Options-  Y on Start system to restricted state (enter)