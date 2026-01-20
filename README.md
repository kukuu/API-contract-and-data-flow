##  API Contract & Data Flow

**5.1 REST API Endpoints**
_Authentication:_
```
  POST   /api/auth/login     → Login with credentials
  POST   /api/auth/refresh   → Refresh JWT token
  POST   /api/auth/logout    → Invalidate token
```
_Alerts:_
```
  GET    /api/alerts         → List alerts with filters
  POST   /api/alerts         → Create new alert
  GET    /api/alerts/{id}    → Get specific alert
  PUT    /api/alerts/{id}    → Update alert
  DELETE /api/alerts/{id}    → Delete alert
```
_Incidents:_
```
  GET    /api/incidents      → List correlated incidents
  POST   /api/incidents      → Create incident from alerts
```
**5.2 WebSocket Channels**

_STOMP Endpoints:_
```
  /ws                         → WebSocket connection
  /app/alerts                 → Send alert updates
  /topic/alerts               → Subscribe to alerts
  /topic/incidents            → Subscribe to incidents
```  
_Message Format:_
```
{
  "type": "ALERT_CREATED",
  "payload": {...},
  "timestamp": "2024-01-15T10:30:00Z"
}

```
**5.3 Data Models Synchronization**

```
// types/index.ts (Frontend)
interface Alert {
  id: string;
  source: '911' | 'SENSOR' | 'SOCIAL_MEDIA';
  priority: 'LOW' | 'MEDIUM' | 'HIGH';
  category: string;
  location: {
    lat: number;
    lng: number;
    address: string;
  };
  timestamp: string;
}

// Alert.java (Backend)
@Entity
public class Alert {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private String id;
    
    @Enumerated(EnumType.STRING)
    private AlertSource source;
    
    @Enumerated(EnumType.STRING)
    private Priority priority;
    
    private String category;
    
    @Embedded
    private Location location;
    
    private Instant timestamp;
}
```
