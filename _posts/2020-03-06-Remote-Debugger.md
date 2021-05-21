0!

We sometimes test on the other devices then our own.
Try the remote debugger if visual studio is not available.

Full information about the remote debugger can found here:
[https://docs.microsoft.com/en-us/visualstudio/debugger/remote-debugging?view=vs-2017](https://docs.microsoft.com/en-us/visualstudio/debugger/remote-debugging?view=vs-2017)


#**Setup the Remote Debugger** 
1) On your own machine go to: `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Remote Debugger`
2) Copy the Remote Debugger folder with all its contents to the C: drive of the target machine (can be saved on a shared folder)
3) On the target machine open the "Remote Debugger" folder where you see the folders x64 and x86, open the x64 folder
4) Now start **msvsmon** by right clicking it and choose **Run as Administrator**
5) You now see a configuration window popping up like in the picture below:
<IMG src="https://docs.microsoft.com/en-us/visualstudio/debugger/media/remotedebuggerconfwizardpage.png?view=vs-2017" alt="Remote Debugger configuration"/>
6) Just click the **Configure remote debugging** button to finish the configuration
7) Now you should see that the Remote debugger is ready and waiting for a connection from your own machine like shown in the picture below.
If not start the msvsmon again as administrator.
![image.png](/.attachments/image-ca731450-9b8c-4d77-87fd-2af1ccd74b5a.png)

#**Connect to the target machine**
1) Go to your own machine
2) Start Visual Studio and make sure you have the same branch checked out as is running on the target branch and have build the solution successfully
3) Now click on `Debug` and choose `Attach to Process`
4) And fill the **computer name with port number** in that is being shown by the Remote Debugger in the description. 
For example PSF-DEV-SHARED3:4022 , see picture below
![image.png](/.attachments/image-d6f809e2-1a92-473b-85bf-bd61b21b5e18.png)
5) Now you're connected to the target machine. When you click on refresh you should see a list of processes running on the target machine. And the Remote debugger should notify that an connection has been established
6) Now you can easily attach to any of the processes by simply choosing your process from the list and click `Attach`



#Event log

If those options are not available try to look if there is a windows error report in the Event Logs from windows 
In this folder : `C:\ProgramData\Microsoft\Windows\WER\ReportQueue\AppCrash_.......`
there are dump files you can open with VS to get some info of wat happened 
