# sensor-data-management

This project provides an API template (using Node.js & Express.js w/ TypeScript) for a sensor data management platform in the marine transportation industry.

This is all in discovery at the moment. Here are my initial ideas:

# Customer API

### Companies

```
interface IPublicCompany {
  companyPublicId: number;
  companyName: string;
  companyLogo?: string; // URL to S3 Bucket
  companyPublicInfo?: string;
}

interface IPrivateCompany {
  companyPublicId: number;
  companyPrivateId: number;
  companyAdmins: number[]; // PublicMate mateId []
  companyCrews: number[]; // PublicCrew crewId []
  companyShips: number[]; // PublicShip shipId []
  companyContactPhoneNumber: string;
  companyContactEmail: string;
  companyPrivateInfo?: string;
  companySecretValue?: string;
  createdBy: number; // PublicMate mateId
}
```

- GET /companies/:companyId - Get A Company's Details
- POST /companies - Create A New Company
- PUT /companies/:companyId - Update A Company's Details
- DELETE /companies/:companyId - Delete An Company
- GET /companies/:companyId/crews - Get A Company's Crews
- GET /companies/:companyId/ships - Get A Company's Ships

### Crews

```
interface IPublicCrew {
  crewPublicId: number;
  crewName: string;
  crewLogo?: string; // URL to S3 Bucket
  crewPublicInfo?: string;
}

interface IPrivateCrew {
  crewPublicId: number;
  crewPrivateId: number;
  crewAdmins: number[]; // PublicMate mateId []
  crewMates: number[]; // PublicMate mateId []
  crewPrivateInfo?: string;
  crewSecretValue?: string;
  isDeployed: boolean;
  createdBy: number; // PublicMate mateId
}
```

- GET /crews/:crewId - Get A Crew's Details
- POST /crews - Register A New Crew
- PUT /crews/:crewId - Update A Crew's Details
- DELETE /crews/:crewId - Delete A Crew
- GET /crews/:crewId/mates - Get A Crew's Mates

### Mates

```
interface IPublicMate {
  matePublicId: number;
  mateFirstName: string;
  mateLastName: string;
  mateProfilePicture?: string; // URL to S3 Bucket
  mateTitle?: string; // Examples: Captain, Deck Officer, Chief Mate, Second Mate
}

interface IPrivateMate {
  matePublicId: number;
  matePrivateId: number;
  mateCrews: number[]; // PublicCrew crewId []
  matePhoneNumber: string;
  mateEmail: string;
  matePrivateInfo?: string;
  mateSecretValue?: string;
  isDeployed: boolean;
}
```

- GET /mates/:mateId - Get A Mates's Details
- POST /mate - Register A New Mate
- PUT /mates/:mateId - Update A Mate's Details
- DELETE /mates/:mateId - Delete A Mate
- GET /mates/:mateId - Get A Mates's Details
- POST /mates/:mateId/sea - isDeployed = true
- POST /mates/:mateId/land - isDeployed = false

### Ships

```
interface PublicShip {
  shipPublicId: number;
  shipName: string;
  shipImages: string[]; // URLs to S3 Bucket
}

interface PrivateShip {
  shipPublicId: number;
  shipPrivateId: number;
  shipAdmins: number[]; // PublicMate mateId []
  shipCrews: number[]; // PublicCrew crewId []
  shipPrivateInfo?: string;
  shipSecretValue?: string;
  isOnVoyage: boolean;
  createdBy: number; // PublicMate mateId
}
```

- GET /ships/:shipId - Get An Ship's Details
- POST /ships - Register A New Ship
- PUT /ships/:shipId - Update A Ship's Details
- DELETE /ships/:shipId - Delete A Ship
- POST /ships/:shipId/depart - isOnVoyage = true
- POST /ships/:shipId/dock - isOnVoyage = false
- GET /ships/:shipId/crews - Get An Ship's Crews

### Devices

```
interface Device {
  deviceId: number;
  deviceName: string;
  deviceType: string;
  deviceSensors: number[]; // Sensor sensorId []
  isProvisioned: boolean;
  lastChecked?: Date;
  checkMessage?: string;
  lastMaintenance?: Date;
  maintenanceMessage?: string;
  scheduledMaintenance?: Date;
  createdBy: number; // PublicMate mateId
}
```

- GET /devices/:deviceId - Get A Device's Details
- POST /devices - Register A New Device
- PUT /devices/:deviceId - Update A Device's Details
- DELETE /devices/:deviceId - Delete A Device
- POST /devices - Provision A Device // Satellite Telemetry System; Internet Communication
- POST /devices/:deviceId/activate - Activate A Device
- POST /devices/:deviceId/deactivate - Deactivate A Device
- POST /devices/:deviceId/updateCheck - Check A Device (Manually Or Programmatically)
- POST /devices/:deviceId/updateMaintainence - Maintain A Device
- POST /devices/:deviceId/scheduleMaintenance - Schedule Maintenance For A Device

### Sensors

This is variable and dependent on needs of specific ships and its cargo.

```
interface Location {
  latitude: number;
  longitude: number;
  cepRadius: number;
}

interface SensorCollectionData {
  sensorCollectionId: number;
  sensorId: number;
  timestamp: Date;
  collectionValue: any; // TODO: Make More Diverse Based On Sensor Schemas
  unit?: string;
  location?: Location;
  description?: string;
  url?: string;
}

interface Sensor {
  sensorId: number;
  deviceId: number;
  sensorName: string;
  sensorType: string; // TODO: Define Generic Types
  sensorCollectionInterval: number; // In Milliseconds
  isActive: boolean;
  lastChecked?: Date;
  checkMessage?: string;
  lastMaintenance?: Date;
  maintenanceMessage?: string;
  createdBy: number; // PublicMate mateId
}
```

- GET /sensors/:sensorId - Get A Sensor's Details
- POST /sensors - Register A New Sensor
- PUT /sensors/:sensorId - Update A Sensor's Details
- DELETE /sensors/:sensorId - Delete A Sensor
- GET /sensors/:sensorId/collectionData - Get A Sensor's Collection Data
- POST /sensors/:sensorId/activate - Activate A Sensor
- POST /sensors/:sensorId/deactivate - Deactivate A Sensor
- POST /sensors/:sensorId/collectionInterval - Change A Sensor's Collection Interval

Examples include:

1. Temperature Sensors
2. Humidity Sensors
3. Atmospheric Gas Sensors
4. Light Sensors
5. Weight Sensors
6. Door Status Sensors
7. Cargo Status Sensors
8. Smart Seals
9. GPS Sensors
10. G-Force Sensors
11. Fuel Level Sensors
12. Engine Performance Sensors
13. RFID Tags
14. Chemical Sensors
15. Cameras (Images & Videos)
16. Microphones (Audio)

An example of a temperature sensor and its data collection:

```
sensorData: SensorData = {
  sensorCollectionId: 4444,
  sensorId: 333,
  timestamp: new Date(),
  rawPayload: "0x010203040506070809"
  collectionValue: 32,
  unit: 'F',
  location: {
    latitude: -0.6394,
    longitude: -90.3372,
    cepRadius: 1,
  }
  description: "It's very strange that it is cold here..."
}

sensor: Sensor = {
  sensorId: 333,
  deviceId: 22,
  sensorName: "Galapagos Temperature Tracker 22"
  sensorType: 'TEMPERATURE',
  sensorCollectionInterval: 60000, // 1 Minute
  isActive: true
}
```

Data Analytics Use Cases

1. Route optimization based on weather conditions and potential delays.
2. Preventative maintenance scheduling based on engine performance data.
3. Cargo spoilage or damage detection through temperature and humidity monitoring.
4. Improved security measures through tamper detection and location tracking.
