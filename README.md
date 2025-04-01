heroku-buildpack-imagemagick
=================================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for vendoring the ImageMagick binaries into your project.

### Features

- Full ImageMagick installation with common formats support
- HEIC/HEIF image format support through libheif
- Configurable versions for both ImageMagick and libheif
- Cached installations for faster deploys

### Install

In your project root:

`heroku buildpacks:add https://github.com/wedsonlima/heroku-buildpack-imagemagick  --index 1 --app HEROKU_APP_NAME`

"index 1" means that imagemagick will be installed first.

### Configuration

You can configure the versions of both ImageMagick and libheif through environment variables:

```bash
# Set ImageMagick version (default: 7.1.1-47)
heroku config:set IMAGE_MAGICK_VERSION=7.1.1-47 -a HEROKU_APP_NAME

# Set libheif version for HEIC support (default: 1.18.2)
heroku config:set LIBHEIF_VERSION=1.18.2 -a HEROKU_APP_NAME
```

### HEIC Support

This buildpack includes HEIC/HEIF image format support through libheif. You can convert HEIC images using commands like:

```bash
convert image.heic image.jpg    # Convert HEIC to JPEG
convert image.heic image.png    # Convert HEIC to PNG
```

Example usage in Ruby:
```ruby
system("convert input.heic output.jpg")
```

Example usage in Node.js:
```javascript
const { execSync } = require('child_process');
execSync('convert input.heic output.jpg');
```

### Changing version
Go to https://www.imagemagick.org/download/releases and find a version you want (*.tar.gz). Edit the `bin/compile` file and change out the version number. Clear cache, as shown below, and redeploy your app to Heroku.

For libheif versions, check the [releases page](https://github.com/strukturag/libheif/releases).

### Clear cache
Since the installation is cached you might want to clean it out due to config changes.

1. `heroku plugins:install heroku-repo`
2. `heroku repo:purge_cache -app HEROKU_APP_NAME`

### Troubleshooting

If you encounter issues with HEIC conversion:

1. Verify ImageMagick installation:
```bash
heroku run "identify -version" -a HEROKU_APP_NAME
```

2. Check HEIC support:
```bash
heroku run "identify -list format | grep HEIC" -a HEROKU_APP_NAME
```

3. If needed, clear the buildpack cache and redeploy.
