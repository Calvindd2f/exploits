AV/EDR evasion
Several AV and EDR evasion techniques are in use:

AMSI bypass: The PowerShell inline script disables AMSI by patching amsi.



AmsiScanBuffer to always return AMSI_RESULT_CLEAN.





DLL unhooking: Since EDR solutions monitor API calls by hooking ntdll, these hooks need to be removed by loading a fresh copy of ntdll from disk and restoring the original section. Otherwise, process hollowing would be detected.
