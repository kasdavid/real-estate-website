# GPS & INTERACTIVE MAP SETUP GUIDE

## Overview
Your Real Estate application now includes complete GPS and interactive mapping functionality. Properties are automatically geocoded and displayed on an interactive Google Maps interface.

## Features Added

### 1. **Interactive Map Page** (`map.html`)
- View all properties on an interactive Google Map
- Filter properties by type
- Search for locations
- Find nearby properties
- Get directions to properties
- View user's current location

### 2. **GPS Functionality** (`js/gps.js`)
- **Geolocation**: Get user's current GPS coordinates
- **Geocoding**: Convert addresses to GPS coordinates
- **Reverse Geocoding**: Convert coordinates to addresses
- **Distance Calculation**: Calculate distance between properties
- **Nearby Search**: Find properties within a specified radius
- **Directions**: Get turn-by-turn directions from Google Maps

### 3. **Enhanced Property Management**
- Automatic address geocoding when adding properties
- GPS coordinates stored with each property
- Distance calculations from user location
- Property markers with info windows on the map

## Setup Instructions

### Step 1: Get a Google Maps API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable these APIs:
   - Maps JavaScript API
   - Geocoding API
   - Places API
4. Create an API key (Credentials → API Keys)
5. Enable billing for your project (required for production)

### Step 2: Add Your API Key

Replace `YOUR_GOOGLE_MAPS_API_KEY` in these files:

**In `map.html` (line 7):**
```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&libraries=places,geometry"></script>
```

**In `add-property.html` (line 227):**
```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&libraries=places,geometry"></script>
```

**In `js/gps.js` (line 5):**
```javascript
const GOOGLE_MAPS_API_KEY = 'YOUR_API_KEY_HERE';
```

### Step 3: Test the Map

1. Navigate to the **Map** link in the navigation menu
2. Click the **"📍 My Location"** button to see your current location
3. Search for a location using the search bar
4. Filter properties by type or radius
5. Click on properties to see details

## How to Use

### Adding Properties with GPS

1. Go to **"Add Property"** page
2. Fill in the property details
3. **Important**: Enter the full address in the **"Address"** field
4. Click **"List Property"**
5. The system will automatically:
   - Geocode the address to get GPS coordinates
   - Store the coordinates with the property
   - Display the property on the map

### Viewing Properties on Map

1. Click the **"📍 Map"** link in navigation
2. Use these features:
   - **Search**: Enter a city, address, or ZIP code
   - **Filter by Type**: Select House, Apartment, or Condo
   - **Radius Filter**: Set search radius in miles
   - **"Nearby" Button**: Find properties near your current location
   - **Click Property**: See details and get directions

### Finding Nearby Properties

1. Click **"📍 Nearby"** button
2. Allow location access when prompted
3. Properties within the radius will be highlighted
4. Shows distance from your location to each property

### Get Directions

1. Click **"Directions"** on any property card on the map
2. Opens Google Maps with turn-by-turn navigation

## File Structure

```
Real Estate/
├── map.html                    # Interactive map page
├── add-property.html           # Updated with GPS
├── js/
│   ├── gps.js                 # GPS & map functionality
│   ├── map-script.js          # Map page logic
│   └── property.js            # Updated with geocoding
├── css/
│   └── map-styles.css         # Map page styling
```

## JavaScript API Reference

### GPSModule Object

All GPS functions are accessible via `window.GPSModule`:

```javascript
// Initialize the map
GPSModule.initializeMap(lat, lng);

// Get user's GPS location
await GPSModule.getUserLocation();
// Returns: { lat: number, lng: number }

// Add user marker on map
GPSModule.addUserMarker(lat, lng);

// Add property marker on map
GPSModule.addPropertyMarker(lat, lng, propertyId, title, price);

// Load all properties on map
GPSModule.loadPropertiesOnMap();

// Geocode an address to coordinates
await GPSModule.getCoordinatesFromAddress(address);
// Returns: { lat, lng, address }

// Calculate distance between two points (in miles)
GPSModule.calculateDistance(lat1, lng1, lat2, lng2);

// Find nearby properties
GPSModule.findNearbyProperties(userLat, userLng, radiusMiles);

// Get directions in Google Maps
GPSModule.getDirections(fromLat, fromLng, toLat, toLng, title);

// Show notification toast
GPSModule.showToast(message, type); // type: 'success' or 'error'
```

## Data Storage

Property data is stored in localStorage with GPS coordinates:

```javascript
{
  id: "prop1703123456",
  title: "Modern Penthouse",
  type: "apartment",
  location: "123 Main St, New York, NY 10001",
  lat: 40.7589,              // Added automatically
  lng: -73.9851,             // Added automatically
  price: 2500000,
  bedrooms: 3,
  bathrooms: 2,
  sqft: 2800,
  amenities: ["Pool", "Gym"],
  // ... other properties
}
```

## Troubleshooting

### "Maps API Key is missing"
- Ensure you've replaced `YOUR_GOOGLE_MAPS_API_KEY` in all three files
- Verify the key is valid and has the required APIs enabled

### Geocoding not working
- Check that the address is valid (e.g., "123 Main St, New York, NY 10001")
- Ensure the Geocoding API is enabled in Google Cloud Console
- Check browser console for error messages

### Locations not appearing on map
- Verify properties have valid addresses
- Check that property addresses can be geocoded
- Look at browser console for geocoding errors

### "Geolocation not supported"
- Geolocation requires HTTPS (except for localhost)
- Some browsers require user permission
- Private browsing may block geolocation

## Performance Tips

1. **Limit properties on map**: Display at most 50-100 markers for performance
2. **Use clustering**: For large datasets, consider adding marker clustering
3. **Cache geocoding**: Don't geocode the same address twice
4. **Optimize images**: Use compressed images for property thumbnails

## Security Notes

⚠️ **Important**: Keep your Google Maps API key secure
- Never commit your real API key to public repositories
- Use environment variables in production
- Restrict API key to your domain in Google Cloud Console

## Future Enhancements

Consider adding:
- Marker clustering for better performance with many properties
- Heat maps showing property density
- Street view integration
- Property comparison on map
- Route optimization (visiting multiple properties)
- Saved favorite locations
- Real-time property alerts based on location

## Support

For issues or questions:
1. Check the browser console (F12) for error messages
2. Verify Google Maps API key is configured
3. Ensure addresses are in valid format
4. Test with a simple address first (e.g., "New York, NY")

---

**Last Updated**: 2026
**Version**: 1.0 GPS Module
