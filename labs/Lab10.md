#### Lab 10 - FortiAIOPS Maps

| Device     | Username/PW        |
| ---------- | ------------------ |
| FortiAIOPS | admin/fortinet4A!! |

> [!NOTE]
> **Why RF maps matter beyond visualisation**
>
> A floor plan in FortiAIOPS is not just a pretty picture — it is the spatial context that makes many AI Insights features meaningful. When FortiAIOPS knows where each AP is physically located relative to walls, floors, and other APs, it can correlate RF metrics with physical space: identify coverage gaps, explain why a roaming event happened at a particular location, predict signal overlap between adjacent APs, and visualise client density heat maps in the context of the actual environment.
>
> **Ekahau integration:**
> Ekahau is the industry-standard tool for professional Wi-Fi planning and site surveys. An Ekahau project (.esx file) contains a full RF model of a building: floor plans, AP placements, wall attenuation values, and predicted coverage. Importing an Ekahau file into FortiAIOPS means you can start with a professionally planned RF model rather than placing APs manually by guesswork. Critically, if the AP names in the Ekahau file match the deployed AP names in FortiAIOPS, the platform automatically correlates the two — placing each real AP at its planned position on the map with zero manual effort.
>
> **Floor plan calibration:**
> Accurate calibration is essential for location-based features. FortiAIOPS uses the dimensions you enter to calculate the real-world scale of the floor plan. An incorrectly calibrated map will produce inaccurate distance calculations, which in turn affects coverage predictions and client location accuracy. Always calibrate using a known fixed distance — structural elements like staircases, elevator shafts, or external walls are ideal as they do not move.

##### Importing a Map

1. Navigate to Wireless > Wi-Fi Maps

  ![alt text](media/lab10-1.png)

2. Import a map
  - Click Import and select the file: AIOPS-TRAINING-MAP.esx

  ![alt text](media/lab10-2.png)

3. Open the file tree to view the imported map

  ![alt text](media/lab10-3.png)

  - The AP is automatically placed because the simulated AP in the Ekahau file matches the name of your deployed AP

##### Renaming

4. Rename the building
  - Click the pipe icon and select Edit

  ![alt text](media/lab10-4.png)

  - Rename the building to HQ

5. Rename the map and floor plan
  - Select AIOPS-TRAINING-MAP, click the : icon and select Edit
  - Rename the map to Fortinet
  - Rename the floor plan to Main

  ![alt text](media/lab10-5.png)

##### Adding a Building

6. Add a new building
  - Create a building called Vancouver, then add a floor and a map

  ![alt text](media/lab10-6.png)

  - Image files in jpg, png, and jpeg formats are supported
  - Provide the dimensions of the floor plan, including any compensation for white outlines
  - Use the following floor plan as your background image

  ![alt text](Floorplan.jpeg)

  - Set the name to Main
  - To calibrate the floor plan, place the blue Measure tool over the elevators and set the distance to 32ft
  - Click Save

  ![alt text](media/lab10-7.png)

##### Placing an AP

7. Place an AP on the map
  - Select Unlock Map in the top left of the map window
  - Select the AP you want to place on the map

  ![alt text](media/lab10-8.png)

  - Click and drag the AP onto the floor plan
  - When satisfied with the location, click Lock Map

> [!NOTE]
> This will grey out the AP from the previous map — this is expected behaviour. An AP can only be actively placed on one floor plan at a time.

8. Observe the band settings
  - Change the band to 5 or 6 GHz depending on the device connected to the AP and observe
  - Scroll through the available options

  ![alt text](media/lab10-9.png)

> [!TIP]
> The band view on the map lets you see predicted coverage for each radio independently. In a real deployment, use this to verify that your 5 GHz coverage reaches all intended areas, and that your 6 GHz cells — which are smaller due to higher frequency propagation characteristics — are positioned correctly for the high-density zones they are designed to serve.

#### Lab complete — move on to Lab 11
