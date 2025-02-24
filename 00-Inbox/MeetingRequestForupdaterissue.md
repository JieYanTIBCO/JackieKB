Hi Steven,

After discussing with the engineering team, we believe another Zoom session is necessary to go through this issue in detail.

Investigation Progress:
We have identified two issues contributing to the problem:

ITM Updaters Attempting to Upgrade an Already Installed Agent

The ITM updaters are trying to upgrade the agent, even though the same version is already running.
To investigate why the updater does not recognize the existing version, we need TRACE-level logging. However, your management team has rejected this test plan. We will revisit this once we obtain view-only persona access.
ITM Updaters Failing to Install the Agent and Retrying, Causing Traffic Issues

We observed permission errors during the agent installation on affected endpoints.
The bundle files are present, but the installation fails.
Since this appears to be a permission issue, we need to check whether the user executing the bundle has elevated privileges.
To confirm, please try running the installation locally using the same user as SCCM instead of deploying it via SCCM.
Meeting Agenda:
Before the Zoom Meeting:
✅ Approval for View-Only Persona:

Please obtain approval from your management team allowing the Proofpoint Support team to access your tenant via persona.
With this approval, you will be able to approve requests from the console, granting our team view-only permissions for your activities and configurations.
✅ Test Machine Preparation:

Identify an endpoint machine for further testing, ensuring that we have administrator privileges on it.

Ensure PSTools is downloaded and installed on this machine. We will use it to simulate the SYSTEM user using the following command:

sh
Copy
Edit
C:\tools\PSTools>psexec -i -s cmd.exe
C:\windows\system32>whoami
nt authority\system
During the Zoom Meeting:
✅ Confirm Support Request Approval

Ensure the support request is approved, granting our team two weeks of view-only access to investigate the issue.
✅ Testing Steps on the Problematic Machine

Manually Install the Agent with SYSTEM Privileges

Run the installation manually as the SYSTEM user (same user as SCCM).
Verify if permission issues occur.
Silent Installation Test with SYSTEM User

sh
Copy
Edit
C:\tools\PSTools>cd C:\tools\winbundle-4.0.1.4002
C:\tools\winbundle-4.0.1.4002>ITMSaaSBundle-4.0.1.4002.exe /install /quiet /norestart contentdetection=1 TargetDir="%ProgramFiles%\IT Client Utility\Client Utility" PreConfigPath="C:\Tools\preconfig.json" /log ITMSaaSBundle_SetupLog.log
Silent Installation Test with SCCM User

Run the installation as the SCCM user and compare results.
✅ Collect Information for Further Analysis

Please let me know when you have obtained approval for view-only persona and prepared the test machine, so we can schedule the Zoom meeting accordingly.

Best regards,
Jackie Yan
ITM Support