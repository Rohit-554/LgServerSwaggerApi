# Liquid Galaxy [Server](https://github.com/LiquidGalaxyLAB/lg-server) Swagger API Documentation

[View Documentation](https://liquidgalaxylab.github.io/LgServerSwaggerApi/)

A comprehensive REST API documentation for controlling and managing Liquid Galaxy systems via SSH. This API enables developers to interact with Liquid Galaxy installations programmatically.

## Overview

The Liquid Galaxy Control API provides endpoints for:

- Managing system operations (reboot, relaunch)
- Controlling visualizations and camera movements
- Displaying custom content (logos, balloons)
- Managing KML file deployments

## Base URL

```
http://localhost:8000
```

## Authentication

All endpoints require SSH configuration parameters:

```json
{
  "ip": "192.168.201.3",
  "port": "22",
  "username": "lg",
  "password": "lg"
}
```

## Available Endpoints

### System Management

- **POST /reboot-lg**: Full system reboot
- **POST /relaunch-lg**: Restart LG software
- **POST /clean-visualization**: Reset all visualizations

### Navigation & Movement

- **POST /start-orbit**: Start orbital movement
- **POST /stop-orbit**: Stop orbital movement
- **POST /flyto**: Navigate to specific coordinates

### Content Display

- **POST /show-logo**: Display logo (left-most rig)
- **POST /clean-logos**: Remove logos
- **POST /show-balloon**: Show information balloon (right-most rig)
- **POST /clean-balloon**: Remove balloons
- **POST /send-kml**: Upload and display KML file

## Examples

Checkout examples [here](https://liquidgalaxylab.github.io/LgServerSwaggerApi/blob/master/examples/examples.txt)

### Sending KML examples
<http://localhost:8000/api/lg-connection/send-kml>

![image](https://github.com/user-attachments/assets/9cf9f467-f2fd-4c08-a5bd-485b11b7bb01)

## Request Body Types

### BasicSSHConfig

```json
{
  "ip": "192.168.201.3",
  "port": "22",
  "username": "lg",
  "password": "lg"
}
```

### RigConfig

Extends BasicSSHConfig with:

```json
{
  "rigs": "3"  // Number of Liquid Galaxy 
}
```

### FlyToConfig

Extends RigConfig with navigation parameters:

```json
{
  "latitude": "40.7128",
  "longitude": "-74.0060",
  "tilt": "45",
  "bearing": "0"
}
```

### LogoConfig & BalloonConfig

Extends RigConfig with:

```json
{
  "kml": "<kml>...</kml>"  // KML content for display
}
```

## Response Formats

### Success Response

```json
{
  "message": "Operation completed successfully",
  "statusCode": 200,
  "data": ""
}
```

### Error Response

```json
{
  "message": "Error description",
  "statusCode": 400,
  "stack": "Error stack trace"
}
```

## Special Notes

1. **Screen Positioning**:
   - Logos appear on the left-most rig
   - Balloons appear on the right-most rig

2. **KML File Upload**:
   - Uses multipart/form-data
   - Files should be in .txt format
   - Requires filename parameter

3. **Parameter Types**:
   - All numeric values should be sent as strings
   - Coordinates must be valid geographic values

## Example Usage

### Flying to a Location

```bash
curl -X POST http://localhost:8000/flyto \
-H "Content-Type: application/json" \
-d '{
  "ip": "192.168.201.3",
  "port": "22",
  "username": "lg",
  "password": "lg",
  "rigs": "3",
  "latitude": "40.7128",
  "longitude": "-74.0060",
  "tilt": "45",
  "bearing": "0"
}'
```

### Displaying a Logo

```bash
curl -X POST http://localhost:8000/show-logo \
-H "Content-Type: application/json" \
-d '{
  "ip": "192.168.201.3",
  "port": "22",
  "username": "lg",
  "password": "lg",
  "rigs": "3",
  "kml": "<kml>...</kml>"
}'
```

## Best Practices

1. Always clean visualizations before starting new ones
2. Validate geographic coordinates before sending
3. Test KML content in single-screen setup first
4. Handle SSH connection errors appropriately
5. Monitor system responses for success/failure

## Development Setup

1. Clone the repository
2. Install dependencies
3. Start local server
4. Access Swagger documentation [here](https://liquidgalaxylab.github.io/LgServerSwaggerApi/)

## API Security Considerations

- Keep SSH credentials secure
- Use environment variables for sensitive data
- Implement rate limiting if needed
- Validate all input parameters

## Support

For issues and feature requests, please create an issue in the repository.

## License

[Add License Information](https://liquidgalaxylab.github.io/LgServerSwaggerApi/blob/master/LICENSE)
