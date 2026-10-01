# Know Your Nzone - Windows Manual Token

Complete Windows manual-token application source, required data and updated illustrated SOP.

## Download the source

Download [Know_Your_Nzone_ManualToken_Windows_Source_With_Updated_SOP.zip](Know_Your_Nzone_ManualToken_Windows_Source_With_Updated_SOP.zip) using GitHub's download button, then extract the ZIP on your Windows development computer. The archive preserves the application's complete folder structure.

Included in the archive:

- `app.py`: application server, manual-token session, automatic state identification, API processing and XLSX reports.
- `static/`: HTML, JavaScript and CSS interface.
- `sop_payload.py`: embedded SOP and layer workbook used by the EXE.
- Updated Windows illustrated SOP and its 11 screenshot assets, including Windows Unblock troubleshooting.
- SOP generation source, Windows executable build configuration and existing project checks.
- Nzone master CSV, India state boundaries and state/layer mapping workbook.
- Developer dependency lists, source manifest and detailed build instructions.

## Build on Windows

Open PowerShell in the extracted source folder. A developer needs Python to edit, run or rebuild the existing source:

```powershell
py -m pip install -r requirements.txt -r requirements-build.txt
.\Package_App.ps1
```

The build refreshes the embedded SOP and creates the executable under `Final App - Ready/Know Your Nzone Manual Token/`. Distribute the complete generated folder as a ZIP. End users do not need Python installed.

For developer use without packaging, run `py app.py` from the extracted source folder.

## Windows SOP

[Download the updated illustrated Windows SOP](Know_Your_Nzone_Manual_Token_SOP.docx). It is also included in the source archive and available from the application interface.

This is the Windows manual-token solution. Tokens, local session history and user-generated reports are not included. The archive contains the current source snapshot; an exact historical source revision for the earlier executable ZIP was not recorded.
