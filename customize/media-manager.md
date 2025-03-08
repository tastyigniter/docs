---
title: "Media manager"
section: customize
sortOrder: 170
---

## Introduction

TastyIgniter provides a flexible and configurable media management system. The Media Manager allows you to upload, organize, and manage media files. By default, Media Manager works with the `storage/app/public/media` directory. It is possible to use external storages such as Amazon S3.

## Configuring external storage

To use the S3 driver, install the required dependencies via Composer.

```bash
composer require league/flysystem-aws-s3-v3 "^3.0" --with-all-dependencies
```

Configure the S3 driver using the `.env` file.

```dotenv
FILESYSTEM_DISK=s3
AWS_ACCESS_KEY_ID=<your-key-id>
AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=<your-bucket-name>
AWS_USE_PATH_STYLE_ENDPOINT=false
```

Using S3 compatible storage services like DigitalOcean Spaces, configure the driver using the `.env` file.

```dotenv
AWS_ENDPOINT=https://nyc3.digitaloceanspaces.com
```

## Media configuration options

The Media Manager in TastyIgniter can be configured according to your preference. You can do this by editing the `config/igniter-system.php` file.

The `config/igniter-system.php` configuration file contains several options that allow you to customize the behavior of the Media Manager:  

- `media`: This section contains the configuration options for the media uploaded files.
  - `disk`: This option specifies the storage disk that the Media Manager should use for uploads. By default, it uses the `public` disk.
  - `folder`: This option specifies the folder within the storage disk where the media files should be stored. By default, it is set to `media/uploads/`.
  - `path`: This option specifies the path to prepend to the file name when generating the file URL. By default, it is set to `media/uploads/`.
  - `max_upload_size`: This option specifies the maximum size (in kilobytes) of the files that can be uploaded through the Media Manager. By default, it is set to 1500 (1.5 MB).
  - `enable_uploads`: This option allows you to enable or disable file uploads. By default, it is set to `true`.
  - `enable_new_folder`: This option allows you to enable or disable the creation of new folders through the Media Manager. By default, it is set to `true`.
  - `enable_rename`: This option allows you to enable or disable the renaming of files and folders through the Media Manager. By default, it is set to `true`.
  - `enable_move`: This option allows you to enable or disable the moving of files and folders through the Media Manager. By default, it is set to `true`.
  - `enable_copy`: This option allows you to enable or disable the copying of files and folders through the Media Manager. By default, it is set to `true`.
  - `enable_delete`: This option allows you to enable or disable the deletion of files and folders through the Media Manager. By default, it is set to `true`.

- `attachment`: This section contains the configuration options for the attachment files.
  - `disk`: This option specifies the storage disk that the Media Manager should use for attachments. By default, it uses the `public` disk.
  - `folder`: This option specifies the folder within the storage disk where the attachment files should be stored. By default, it is set to `media/attachments/`.
  - `path`: This option specifies the path to prepend to the file name when generating the file URL. By default, it is set to `media/attachments/`.

These options provide a high level of control over how media files are handled in your application. You can customize these options according to your application's requirements.

To publish the config file to `config/igniter-system.php` run:

```bash
php artisan vendor:publish --tag="igniter-config"
```

For more information on configuring external storage, check out <a href="https://laravel.com/docs/filesystem#configuration" target="_blank">Laravel's filesystem docs</a>.

## Defining media fields

Media fields allow you to choose or attach images, videos, and other media files from the library. You can define media fields in the configuration file of your model and use them in forms.

To use a media field in a form, you can use the `mediafinder` form widget. For a single image field, you can use the `mediafinder` form widget with the `isMulti` set to `false`:

```php
return [
    'fields' => [
        'featured_image' => [
            'label' => 'Featured Image',
            'type' => 'mediafinder',
            'isMulti' => false,
        ],
    ],
];
```

For a multiple images field, you can use the `mediafinder` form widget with the `isMulti` set to `true`:

```php
return [
    'fields' => [
        'gallery_images' => [
            'label' => 'Gallery Images',
            'type' => 'mediafinder',
            'isMulti' => true,
        ],
    ],
];
```

## Attaching media fields to a model

Media files can be attached to a model using the `Igniter\Flame\Database\Attach\HasMedia` trait and defining the `$mediable` property. For example, to attach single media files to the `gallery_images` field of `MenuItem` model, you can use the following code

```php
class MenuItem extends Model
{
    use \Igniter\Flame\Database\Attach\HasMedia;

    public array $mediable = ['featured_image'];
}
```

To attach multiple media files to the `gallery_images` field of `MenuItem` model, you can use the following code:

```php
class MenuItem extends Model
{
    use \Igniter\Flame\Database\Attach\HasMedia;

    public array $mediable = ['gallery_images' => ['multiple' => true]];
}
```

## Displaying media on your site

To display a media file in a template, you can use the `media` method of the media model.

For a single media field, you can display the image using the following code:

```blade
<img src="{{ $model->featured_image }}" alt="Featured Image">
```

For a multiple media field, you can display the images using a loop:

```blade
@foreach($model->gallery_images as $media)
    <img src="{{ $media }}" alt="Gallery Image">
@endforeach
```

For a single attached media field, you can display the image using the following code:

```blade
<img src="{{ $model->featured_image->getThumb() }}" alt="Featured Image">
```

For a multiple attached media field, you can display the images using a loop:

```blade
@foreach($model->gallery_images as $media)
    <img src="{{ $media->getThumb() }}" alt="Gallery Image">
@endforeach
```

The `getThumb` method accepts an optional parameter to specify the size of the thumbnail. For example, to get a thumbnail of size 200x200, you can use:

```blade
<img src="{{ $media->getThumb([
    'width' => 200,
    'height' => 200,
]) }}" alt="Gallery Image">
```
