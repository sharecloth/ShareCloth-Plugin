# Plugin API

## Installation

1. Include js and css
2. Init plugin

Here is a sample code:

```html
<!doctype html>
<html>
<head>
    <title>Sharecloth web viewer example</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes"/>
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
    <meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0, shrink-to-fit=no">

    <!-- include stylesheet -->
    <link rel="stylesheet" type="text/css" href="http://plugin-web.globedrobe.com/3d-plugin-gl/0.6.1/plugin.css"/>

    <!--- plugin uses jquery lib -->
    <script src="https://code.jquery.com/jquery-1.12.4.min.js" integrity="sha256-ZosEbRLbNQzLpnKIkEdrPv7lOy9C27hHQ+Xp8a4MxAQ=" crossorigin="anonymous"></script>
</head>
<body>

<div align="center" id="plugin-3d"></div>
<!-- connect web plugin viewer -->
<script type="text/javascript" src="http://plugin-web.globedrobe.com/3d-plugin-gl/0.6.1/js/3d-client.min.js"></script>

<script type="text/javascript">

    $(document).ready(function () {
        var plugin = new Plugin3d($('#plugin-3d'),
            {
                'host': 'http://plugin-web.globedrobe.com/api',
                'images': 'http://plugin-web.globedrobe.com/3d-plugin-gl/0.6.1/images/'
            });

        // load scene
        plugin.loadScene(13, function() {
            plugin.loadDummy('44d7c064-be86-446f-adba-b2bbb76ae8fa', 'VPose', function() {
                plugin.loadProducts('c-f71a7127-956d-4433-97b2-47a966a37db3', 'VPose', 0, function() {
                    plugin.hideCurtains();
                });
            });
        });
    });
</script>
</body>

</html>
```


### Initialisation parameters
```javascript
var plugin = new Plugin3d($('#plugin-3d'),
    {
        // server with DATA API
        'host': 'http://plugin-web.globedrobe.com/api',
        // path to images
        'images': 'http://plugin-web.globedrobe.com/3d-plugin-gl/0.5/images/',
        // sizes of plugin window
        'width'  : 480,
        'height' : 600,
        // vertical fov
        'fov'    : 45,
        // dimensions (in meters) / see https://msdn.microsoft.com/en-us/library/ms924585.aspx 
        'near'   : 0.1,
        'far'    : 10,
        // color and opacity for backgrounds
        'clearColor' : 0xcccccc,
        'transparent' : false,
        // animation time (ms)
        'curtainsDelay' : 1000,
        // additinal buttons
        'extendedCameraControls' : true,
        // disable pith
        'disablePitch' : false,
        // error on initialization process (for example - WebGL is not supporte by browser)
        'error' : function (e) { console.error (e); }
    });
```

## API methods

### Load Scene

Example:
```javascript
plugin.loadScene(sceneId, callback);
```

Where:
- `sceneId` - ID of a scene. At this time, there is two main scenes: `8` - gray scene, `13` - white scene
- `callback` - callback functions calls when a scene is loads


### Load dummy

Loads avatar to scene.

Example:
```javascript
plugin.loadDummy(avatarId, pose, callback);
```

Where
- `avatarId` - avatar ID. Can be taken with sharecloth data api.s
- `pose` - you can use one of this standard poses (only for female avatars): `VPose`,
`FashionPose02`, `FashionPose17`, `FashionPose18`
- `callback` - callback function calls when avatar is completely loads

### Hide and show curtains

Two helper methods, that can be used to toggle scene curtains.

Example:

```javascript
plugin.showCurtains();
plugin.hideCurtains();
```


### Load Products

This is the main function, that's load cloth calculation result;

Example:
```javascript
plugin.loadProducts('c-' + productId, pose, clothType, callback);
```

Where:
- `productId` - can be retrieved from sharecloth data api
- `pose` - avatar pose. One of  `VPose`, `FashionPose02`, `FashionPose17`, `FashionPose18`
- `clothType` - not used at this time. Can be pass `0`.
- `callback` - function, than executes after cloth is load to scene 
