INFRA INVENTORY - portable, no install
======================================

Start
  1. Unzip anywhere (a network share works too).
  2. Open index.html in Edge or Chrome. Nothing else is needed.
  3. Click "Connect project folder" and pick this InfraInventory folder.
     Once per visit (the browser remembers it, you only confirm access).
     The page then loads Inventory\inventory.xlsx and, on every
     "Save to Excel", writes the workbook in place AND refreshes index.html
     by itself. No build script is needed.
  4. Add data in the page, or fill the sheets in Excel and use "Reload from Excel".
  5. Settings: add your operators, environments and rack device types first.

Folders
  index.html              the dashboard (what you open)
  dashboard-template.html copy of the page, used to refresh index.html on Save. Keep it.
  Inventory\              inventory.xlsx = the database
  Issues\                 reserved for DC Open Issues files
  Stuff\                  files shown on the Stuff tab
    KnowledgeBase\<KB-0001>\   attachments of one article
    OpenIssues\<OI-0001>\      attachments of one open issue
  vendor\                 Excel library (SheetJS), so the page works offline

How it works
  A) Normal: Connect project folder, work in the page, Save to Excel.
     inventory.xlsx and index.html are both updated. Others open index.html.
  B) If you edit inventory.xlsx directly in Excel: open index.html, confirm
     access to the project folder once, and the newer workbook is loaded by
     itself (or use "Reload from Excel"). The next Save to Excel refreshes
     index.html for everyone.
  Without a connected folder the page still works: Open Excel..., Save to Excel.

Good to know
  - Use Edge or Chrome. Other browsers can only download the saved file.
  - If you leave the page with unsaved changes, the browser asks first.
  - If the Excel file changed on disk after the page loaded it, Save asks you to
    click once more before it overwrites the file.
  - Edits that were not saved to Excel stay in this browser.
  - Photos pasted into a DC Open Issue are saved as files in Issues\<ID>\ when the
    project folder is connected, and their file names are recorded in the workbook
    (sheet IssueComments, column Photos). Photos added before the folder was
    connected stay in this browser and move into the folder when you connect it.
  - Adding a file on the Stuff / Knowledge base / Open Issues tabs writes into the
    real folder only after you click "Connect ... folder" and pick it. Without
    that, the page records the name and you copy the file into the folder yourself.
  - New columns added in the page are written to the workbook (and to the _Schema
    sheet) on the next Save to Excel. No file editing is needed.
  - Do not rename the sheets or the header names in the first row of a sheet.
  - DC checks are optional: the DC Checks tab is a log, nothing reminds you.
