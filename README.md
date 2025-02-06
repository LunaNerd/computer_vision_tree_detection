# Computervisie-g10

#### Groepsleden:

Dirven, Thomas<br>
Van Looy, Robbe<br>
Verbiest, Wout<br>
Versmissen, Luna<br>
Yüksel, Ilkay<br>

## TLDR;

Deze repo werd gebruikt voor het project van het vak Computervisie.

Het doel van dit project bestond uit het identificeren, localiseren en de breedte meten van bomen op videos gemaakt vanop een brommer. Studenten waren vrij om een aanpak te kiezen. Wij kozen voor een aanpak gebruik makende van:  
- [PercepTreeV1​](https://github.com/norlab-ulaval/PercepTreeV1) (een Mask R-CNN model getrained op synthetische data)
- [COLMAP](https://colmap.github.io/index.html) ( Structure-from-Motion (SfM) en Multi-View Stereo (MVS) CLI/GUI software)
  
Een visualisatie van ons resultaat en een oplijsting van de bijhorende breedtebepalingen:

| Beschrijving | resultaat |
|-------------|------|
| Gif van output van het model op 200 frames (eerste 30 seconden) | ![output_video_eastbound_200_frames-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/a4c9fab2-ec98-44af-b06a-416a91c99c18) |
| De resultaten (breedte bepaling, locatie en aantal metingen per boom) | [results_improved.csv](results_improved.csv) |
  
##### Deze repo staat vol met files, MAAR  `project_cv_based_tree_measurement.ipynb` is de enige file die  alle opgeschoonde code van heel het project bevat.

## Methode

#### Preprocessing​

| Calibration:                                                                              | Undistortion                                                                              |
|-------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| ![image](https://github.com/user-attachments/assets/840b51c7-b4e5-4b5c-8b3d-e62758a3b9a2) | ![image](https://github.com/user-attachments/assets/861e00a7-a915-459d-a945-3595ca9d71da) |

#### Tree Detection​
Masks, keypoints en bounding boxes plaatsen rond bomen op frames van de video met behulp van PerceptreeV1 (een Mask R-CNN model getrained op synthetische data).  

| Perceptree train-afbeeldingen illustratie: | Perceptree results op onze frames:  | Keypoint betekenis                                                                            |
|--------------------------------------------|-------------------------------------|-----------------------------------------------------------------------------------------------|
| ![syn_data](https://github.com/user-attachments/assets/56bbc04e-b473-46af-90f6-42f6fb6d03f0) | ![our_data](https://github.com/user-attachments/assets/5c9d030a-d102-401a-a8b0-1ff1a4bac256) | ![keypoints](https://github.com/user-attachments/assets/3c73943a-1839-4ac6-ae5a-8dde745c9db8) |

#### Triangulation​

Met behulp van COLMAP.  

| Stap 1: Structure-From-Motion​: Resultaat is een "sparse 3D map" | Stap 2: Multi-View Stereo: Resultaat is een diepteafbeelding voor elke frame |
|------------------------------------------------------------|-------------------|
| ![image](https://github.com/user-attachments/assets/3f9890cc-86e8-4e23-942e-3c92446cd7f3) | ![image](https://github.com/user-attachments/assets/f0ad0340-21be-4655-8228-281455834560) |
| Elk punt op deze 3D point cloud bevat een link naar de afbeeldingen waaruit het punt bepaald werd en de pixel locatie van dat 3D punt op de respectievelijke afbeelding   | - |

#### Tree Mapping​

In deze stap identificeren we bomen overheen frames. Dit deden we door de "Felling Cut"-keypoints gevonden op een frame door PerceptreeV1 (2D) om te zetten naar 3D coordinaten (mbv van de sparse 3D map) en vervolgens deze punten te clusteren. Elke boom krijgt een id toegewezen.

| De 3D locatie van elke Perceptree keypoint (Felling Cut) na clustering (elke cluster heeft een kleur) op een deel van de video | Dezelfde boom in opeenvolgende frames behoudt zijn kleur |
|------------------------------------------------------------|--------|
| ![image](https://github.com/user-attachments/assets/98ee1643-2bea-4763-adf4-c3bb78b224b7) | ![image](https://github.com/user-attachments/assets/fa989bdf-b573-40dc-b58a-3a15786c37d0) ![image](https://github.com/user-attachments/assets/925fcf7f-a637-4589-8009-8d1d719c61cb) |

| De geclusterde locatie van elke boom in de volledige video (geprojecteerd op een 2D vlak evenwijdig met de straat) |
|------------------------------------------------------------|
| ![image](https://github.com/user-attachments/assets/b46aa1bc-6fab-4259-9a3f-91fd6e8b64fb) |

#### Tree Width Estimation​

We gebruiken de keypoints (Diameter Left en Right Point) van PerceptreeV1 in combinatie met de diepteafbeeldingen van de "triangulation"-stap om de breedte van elke boom te bepalen overheen verschillende frames.

| De keypoints op een diepte afbeelding | Een gebruikte grafiek bij het berekenen van de breedte |
| --- | --- |
| ![image](https://github.com/user-attachments/assets/4c70be99-0f06-4afc-816c-b71358a66e9c) |  ![image](https://github.com/user-attachments/assets/c9cb548f-5490-4530-bc2f-8c4cc70d68ee) |

#### Postprocessing​

We maakten een lijst van het gemiddelde en de standard deviation van de metingen van elke boom. Zo kan ook de accuraatheid van ons resultaat ingeschat worden.

Zie: [results_improved.csv](results_improved.csv)

## File structure
```
Computervisie-g10/
├─ assets/
│  ├─ original/
│  │  ├─ contains the original undistorted video's
│  ├─ extracted_05/
│  │  ├─ eastbound_20240319/
│  │  │  ├─ contains the frames of the eastbound video
│  │  ├─ westbound_20240319/
│  │  │  ├─ contains the frames of the westbound video
│  ├─ extracted_30/
│  │  ├─ calibration/
│  │  │  ├─ contains the frames of the calibration video
│  ├─ undistorted_05/
│  │  ├─ eastbound/
│  │  │  ├─ contains the undistorted frames of the eastbound video
│  │  ├─ westbound/
│  │  │  ├─ contains the undistorted frames of the westbound video
│  ├─ annotated_05/
│  │  ├─ eastbound/
│  │  │  ├─ contains information about the detected trees
│  │  ├─ westbound/
│  │  │  ├─ contains information about the detected trees
│  ├─ colmap/
│  │  ├─ colmap/
│  │  │  ├─ contains the repository of colmap
│  │  ├─ colmap_build/
│  │  │  ├─ contains the build of colmap
│  │  ├─ workspaces/
│  │  │  ├─ westbound/
│  │  │  │  ├─ sparse/
│  │  │  │  ├─ sparse_aligned/
│  │  │  │  ├─ dense/
│  │  │  ├─ eastbound/
│  │  ├─ reconstruction/
│  │  │  ├─ eastbound/
│  │  │  │  ├─ sparse_txt/
│  │  │  │  ├─ dense_txt/
│  │  │  ├─ westbound/
├─ project_cv_based_tree_measurement.ipynb (deze file bevat alle code van heeel het project opgeschoond)
```
