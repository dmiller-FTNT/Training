# Lab 1 Prepwork

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1!   |
| FortiManager       | admin/fortinet4A!!  |

### Task 1 - Check on FabricStudio
1. Log into the FabricStudio
2. Navagate to Fabric Workspace > fabric id 2
3. Check that all devices are running
4. Login into Fortimanager
5. Verify the following on FGTDC01 are correct for your pod (xx is Pod Nunber eg 02 for pod2)
   1.  The IP of Port2 (172.31.1xx.50)
   2.  The static default route (172.31.1xx.254) 
   3.  The Pod_id variable for FGTDC01 is your pod id xx
       -  Right Click on the device > Edit Variable Mapping

>[!EXAMPLE]
>For example Pod2 would look like the following
>![](media/Lab1-1.png)
>![](media/Lab1-2.png)
>![](media/Lab1-6.png)

>[!NOTE]
>These should already be correct, but humans had to change these so ...

### Task 2 - Branch FGT ZTP

6. Under Device Manager add a new Model device for your Bench Fortigate with the following configuration
   - Name : FGTBr01
   - Serial Number : [ your device ]
   - Device Model : [ Will populate automaticly ]
   - Add to device group : Branches
   - Assign Policy Package : FGTBranch
   - Provisioning Templates : Branch_Group
   - Edit Meta variables and change the mapping value for
     - pod_ip : [ your pod nummber ] eg 02
     - wan_interface : wan1
   - Leave the rest default

    ![](media/Lab1-3.png)
    ![](media/Lab1-4.png)

7. Connect your fortigate's WAN1 to the lab connection and power the device up
8. Wait for the device to autolink, once complete run and syncronized, run a quick install
   - This might show that nothing is to be installed, thats fine 
    ![](media/Lab1-5.png)

#### Lab complete move onto Lab 2

>[!NOTE]
>As this is a wireless training Lab most of the initial templates have been configured for you, if you're curious please take the time to look through them as they likely **won't** be touched on in later labs 


