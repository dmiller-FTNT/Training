# Lab 1 Prepwork

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1! |
| FGTDC01      | admin/fortinet4A!! |

### Task 1 - Check on FabricStudio
1. Log into the FabricStudio
2. Navagate to Fabric Workspace > fabric 2
3. Check that all devices are running
4. Login into Fortimanager
5. Verify the following on FGTDC01 are correct for your pod (xx is Pod Nunber eg 02 for pod2)
   1.  the IP of Port2 (172.31.1xx.50) and 
   2.  the static default route (172.31.1xx.254) 

   - For example Pod2 would look like the following
  ![](media/Lab1-1.png)
  ![](media/Lab1-2.png)

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