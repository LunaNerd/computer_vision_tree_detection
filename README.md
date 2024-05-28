# Computervisie-g10

Deze repo zal worden gebruikt voor het project van het vak Computervisie.

Groepsleden:

Dirven, Thomas<br>
Van Looy, Robbe<br>
Verbiest, Wout<br>
Versmissen, Luna<br>
Yüksel, Ilkay<br>

### Geautomatiseerde Boomkartering en -meting met Computer Vision

Welkom in onze projectrepository, waar we de manier waarop we de groei en meting van bomen monitoren willen revolutioneren. Dit project wordt ontwikkeld door een team van 5 studenten onder leiding van David Van Hamme, met als doel een innovatieve oplossing te vinden voor nauwkeurige boomkartering en het monitoren van seizoensgebonden groei door middel van computer vision.

#### Projectoverzicht
In verschillende sectoren, zoals bosbeheer, commerciële houtproductie en milieubescherming, is het nauwkeurig in kaart brengen en monitoren van bomen cruciaal. Traditionele methoden omvatten ofwel grootschalige luchtbewaking of arbeidsintensieve grondmetingen, elk met zijn beperkingen. Ons project stelt een volledig geautomatiseerde, op camera's gebaseerde oplossing voor om bomen te detecteren, in kaart te brengen en te meten, met een focus op de meting van de stamdiameter op borsthoogte (DBH) door middel van periodieke meetcampagnes.

#### Doelstellingen
Nauwkeurige detectie van bomen in videosequenties.
3D-lokalisatie van gedetecteerde bomen om hun posities in kaart te brengen.
Meting van de stamdiameter om de groei te monitoren.


## file structure
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
├─ project_cv_based_tree_measurement.ipynb
```