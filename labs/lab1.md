# Lab 1 Prepwork

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1! |
| FGTDC01      | admin/fortinet4A!! |

### Task 1 - Check on FabricStudio
1. Log into the FabricStudio
2. Navagate to Fabric Workspace -> fabric id 2
3. Check that all devices are running
4. Login into Fortimanager
5. Verify the following on FGTDC01 are correct for your pod (xx is Pod Nunber eg 02 for pod2)
   1.  the IP of Port2 (172.31.1xx.50) and 
   2.  the static default route (172.31.1xx.254) 

   - For example Pod2 would look like the following
  ![](media/Lab1-1.png)
  ![](media/Lab1-2.png)

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

9. Lab complete move onto lab 2

>As this is a wireless training Lab Most of the initial templates have been configured for you, if you're curious please take the time to look through them as they **won't** be touched on in later labs 
