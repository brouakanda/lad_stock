# lad_stock
projet de fin de module java by BROU Akanda assi aristide / LENAUD Nadia / DOUMBIA Karidjatou*
#  LAD Stock CI — Application de Gestion des Stocks

**INP-HB — Département Informatique IC-GL | Année 2025-2026**

---

##  Équipe de développement

| Nom complet | Responsabilités principales |
|---|---|
| **BROU AKANDA ASSI ARISTIDE** | Architecture MVC, ProduitDAO, MouvementDAO, MainApp, SplashScreen |
| **LENAUD SOKONI NADIA** | Login, SessionManager, ExportUtil (XLSX+PDF), DashboardController, Tests Produit |
| **DOUMBIA KARIDJATOU** | Modèles, CategorieDAO, FournisseurDAO, UtilisateurDAO, AlertUtil, Tests |

---

##  Technologies utilisées

| Technologie | Version |
|---|---|
| Java | 21 (LTS) |
| JavaFX | 21 |
| MySQL | 8.x |
| Maven | 3.9+ |
| Apache POI (XLSX) | 5.2.5 |
| iTextPDF (PDF) | 5.5.13.3 |
| JUnit 5 | 5.10.0 |

---

##  Architecture du projet

```
stockmanager-ci/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/com/inphb/icgl/stocks/
    │   │   ├── MainApp.java              ← Point d'entrée
    │   │   ├── SplashScreen.java         ← Écran de démarrage
    │   │   ├── model/                    ← MODÈLE : POJOs avec JavaFX Properties
    │   │   │   ├── Utilisateur.java
    │   │   │   ├── Categorie.java
    │   │   │   ├── Fournisseur.java
    │   │   │   ├── Produit.java
    │   │   │   └── Mouvement.java
    │   │   ├── repository/               ← INTERFACES Repository
    │   │   │   ├── ICategorieRepository.java
    │   │   │   ├── IFournisseurRepository.java
    │   │   │   ├── IProduitRepository.java
    │   │   │   ├── IMouvementRepository.java
    │   │   │   └── IUtilisateurRepository.java
    │   │   ├── dao/                      ← IMPLÉMENTATIONS SQL (PreparedStatement)
    │   │   │   ├── CategorieDAO.java
    │   │   │   ├── FournisseurDAO.java
    │   │   │   ├── ProduitDAO.java
    │   │   │   ├── MouvementDAO.java
    │   │   │   └── UtilisateurDAO.java
    │   │   ├── controller/               ← CONTRÔLEURS FXML
    │   │   │   ├── LoginController.java
    │   │   │   ├── MainLayoutController.java
    │   │   │   ├── DashboardController.java
    │   │   │   ├── CategorieController.java
    │   │   │   ├── FournisseurController.java
    │   │   │   ├── ProduitController.java
    │   │   │   ├── MouvementController.java
    │   │   │   └── UtilisateurController.java
    │   │   └── utils/                    ← UTILITAIRES
    │   │       ├── DatabaseConnection.java  ← Singleton JDBC
    │   │       ├── SessionManager.java      ← Utilisateur connecté
    │   │       ├── PasswordUtil.java        ← Hashage SHA-256
    │   │       ├── AlertUtil.java           ← Boîtes de dialogue
    │   │       └── ExportUtil.java          ← Export XLSX + PDF
    │   └── resources/
    │       ├── fxml/                     ← VUES Scene Builder
    │       │   ├── SplashScreen.fxml
    │       │   ├── Login.fxml
    │       │   ├── MainLayout.fxml
    │       │   ├── Dashboard.fxml
    │       │   ├── Categorie.fxml
    │       │   ├── Fournisseur.fxml
    │       │   ├── Produit.fxml
    │       │   ├── Mouvement.fxml
    │       │   └── Utilisateur.fxml
    │       └── css/
    │           └── styles.css            ← Design vert forêt + or ivoirien
   
```

---

##  Installation et démarrage

### 1. Prérequis
- **Java 21** (JDK)
- **MySQL 8.2** en cours d'exécution
- **Maven 3.9+**
- **Scene Builder 21** (pour éditer les FXML)

### 2. Créer la base de données

```bash
mysql -u root -p < stockmanager_ci.sql
```

Ou dans MySQL Workbench, exécuter le fichier `stockmanager_ci.sql`.

### 3. Configurer la connexion

Modifier `src/main/java/com/inphb/icgl/stocks/utils/DatabaseConnection.java` :

```java
private static final String URL      = "jdbc:mysql://localhost:3306/stockmanager_ci...";
private static final String USER     = "root";       // ← votre utilisateur MySQL
private static final String PASSWORD = "";           // ← votre mot de passe MySQL
```

### 4. Compiler et lancer

```bash
# Compiler
mvn clean compile

# Lancer directement avec Maven
mvn javafx:run

# Ou créer le JAR et lancer
mvn clean package
java -jar target/stockmanager-ci-1.0.jar
```

### 5. Lancer les tests

```bash
mvn test
```

---

##  Comptes par défaut

| Login    | Mot de passe   | Rôle |
|----------|----------------|---|
| `admin`  | `atta123`      | ADMIN |
| `brou`   | `aristide2026` | GESTIONNAIRE |
| `lenaud` | `nadia2026`    | GESTIONNAIRE |

---

## Packaging exécutable (jpackage)

```bash
# Compiler le JAR
mvn clean package

# Générer l'installeur Windows (.exe)
jpackage --input target/ \
         --name StockManagerCI \
         --main-jar stockmanager-ci-1.0.jar \
         --main-class com.inphb.icgl.stocks.MainApp \
         --type exe \
         --app-version 1.0

# Linux (.deb)
jpackage --input target/ \
         --name StockManagerCI \
         --main-jar stockmanager-ci-1.0.jar \
         --main-class com.inphb.icgl.stocks.MainApp \
         --type deb \
         --app-version 1.0
```

---

##  Fonctionnalités implémentées

| Module | Status | Détails |
|---|--|---|
| Splash Screen |  | Stage UNDECORATED, PauseTransition 3s, fade-out |
| Authentification |  | SHA-256, blocage 3 tentatives, rôles |
| Dashboard |  | 4 indicateurs, top 5 alertes |
| Catégories |  | CRUD + pagination + recherche temps réel |
| Fournisseurs |  | CRUD + pagination + recherche nom/tél |
| Produits |  | CRUD + alertes rouges + combobox dynamiques |
| Mouvements |  | Entrées/sorties + mise à jour auto + filtres date |
| Export XLSX |  | Apache POI, couleurs, FileChooser |
| Export PDF |  (BONUS) | iTextPDF, tableau coloré |
| Utilisateurs |  | Admin only, CRUD, désactivation |
| Pagination |  | LIMIT/OFFSET sur tous les TableView |
| Tests JUnit 5 |  (BONUS) | 30 tests (CategorieDAO + ProduitDAO + UtilisateurDAO) |

---

##  Charte graphique

- **Couleur principale** : Vert forêt `#1B5E20` (inspiré des couleurs ivoiriennes)
- **Accent** : Or `#F9A825`
- **Alerte** : Rouge `#C62828` / Rose pâle `#FFEBEE`
- **Fond** : Gris clair `#F5F6FA`
- **Police** : Segoe UI / DejaVu Sans

---

*INP-HB — Département Informatique IC-GL 2025-2026*
