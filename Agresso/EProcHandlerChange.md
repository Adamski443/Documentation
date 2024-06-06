# EProcHandler Change Request Production Steps

Take a snapshot of the web and app servers before changes:

1. Create a backup directory and copy relevant files into this directory on both web and app servers.
2. Park intelagent "Marketplace Purchase Orders - generate PO01 for XML POs" so that no MP Pos send during the change.
3. Park Scheduled Job "UECEX backup in case any xml files unsent" so that no MP Pos send during the change.
4. In Marketplace go to Setup > General Site Settings > Document Export Settings under Schedules click on Suspend and confirm.
5. Web Server - stop the AGRUKIsolator Application Pool
6. Web Server - copy the new fixed files into relevant directories for Isolator
7. Web Server - make sure you replace the default web.config file with the correct web.config file
8. App Server - stop the EPHandler Service
9. App Server - uninstall the EPHandler Service
10. App Server - Reboot the app server
11. App Server - copy the new fixed files into relevant directories for EPHandler
12. App Server - make sure you replace the AGRUKePHandler.exe.config file with the correct version
13. App Server - Re-install EProchandler service from the `C:\Program Files (x86)\UNIT4 Business World On! (v7)\Bin` location usin cmd as admin
14. App Server - Start EPHandler service - check log files that it's listening
15. Web Server - start the AGRUKIsolator Application Pool (may need to do an iisreset)
16. Test you can send an invoice to the Isolator and it forwards to the Handler via Postman
17. Test you can send a PO to the Marketplace via Agresso (for live send a PO that is a duplicate as all we are testing is response back from marketplace so a rejection is fine)
18. Unpark intelagent "Marketplace Purchase Orders - generate PO01 for XML POs" so that MP Pos can now send
19. Unpark Scheduled Job "UECEX backup in case any xml files unsent" so that MP Pos can now send
20. In Marketplace go to Setup > General Site Settings > Document Export Settings under Schedules click on Resume and confirm, so that invoices will again send from Marketplace

If all ok, delete snapshots.
