# Image processing {#concept_m4f_dcn_vdb .concept}

This topic describes the basic features and versions of Alibaba Cloud OSS Image Processing \(IMG\) service and how to use this service. Featuring high capacity and cost efficiency, IMG provides two image processing APIs that allow you to build image-related services.

**Note:** 

IMG is activated automatically when you activate OSS. For information about how to activate OSS, see [Sign up for OSS](../../../../reseller.en-US/Quick Start/Sign up for OSS.md#).

## Basic features {#section_rqv_sdo_b3e .section}

-   Query image metadata
-   Convert image formats
-   Scale, crop, and rotate images
-   Attach images, text, and text-and-image watermarks to images
-   Customize image processing styles
-   Call multiple image processing features by using pipelines

## Versions {#section_m69_ymd_uxf .section}

There are two versions of IMG.

**Note:** This topic describes the features of the latest version of IMG. The API of the previous version will no longer receive updates. For details related to compatibility, see [FAQ on using old and new versions of APIs and domain names](reseller.en-US/Data Processing/Image Processing/FAQ on using old and new versions of APIs and domain names.md#).

## Quick start {#section_qqp_nor_rzd .section}

-   Create an image style by completing the following steps:
    1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
    2.  Click your bucket name to go to the Overview page of the bucket.
    3.  Choose the **Image Processing** tab, and then click **Create Rule**.
    4.  On the Create Rule page, configure the image style by performing graphical operations on the **Basic Edit** tab page or by using an SDK or parameters on the **Advanced Edit** tab page.

        The parameters on the **Basic Edit** tab page are as follows:

        -   Rule Name: the name of the image style. The name must be 1 to 64 characters in length, and can contain numbers, letters, underscores \(\_\), hyphens \(-\), and periods \(.\).
        -   Format: the format of images. Values: Original | jpg | jpeg | png | bmp | gif | webp | tiff.
        -   Fade In: Specifies whether to enable the Fade In function.
        -   Adaptive Orientation: Specifies whether to enable the Adaptive Orientation function.

            We recommend that you enable the Adaptive Orientation function. When this function is enabled, an image is rotated according to its EXIF data before it is resized.

        -   Image Quality: the quality of images. Values: Relative | Absolute | Uncompressed.
        -   Resize Type: the method of resizing images. Values: Thumbnail Disabled | Proportional Scale Down | Proportional Scale Up | Fixed Width and/or Height.

            **Note:** The "long side" refers the side with a larger source size to target size ratio. The same applies to the "short side." For example, for an original image that is scaled from 400x200 to 800x100, the original-to-target ratios are 0.5 \(400/800\) and 2 \(200/100\). Given that 0.5 is less than 2, the 200 side is the longer side and the 400 side the shorter one.

        -   Image Brightness: the image brightness.
        -   Image Contrast: the level of contrast in an image.
        -   Image Sharpening: Specifies whether to sharpen an image. If this function is enabled, the level of sharpening applied to an image.
        -   Image Blurring: Specifies whether to blur an image. If this function is enabled, the level of blur effect applied to an image.

            When the Image Blurring function is enabled, you can set Blur Radius and Blur Sigma.

        -   Image Rotation: the angle of rotation of an image.
        -   Watermark: Specifies whether to enable the Watermark function. If this function is enabled, the type of watermark added to an image.
        The parameters on the **Advanced Edit** tab page are as follows:

        -   Rule Name: the same as the Rule Name parameter on the **Basic Edit** tab page.
        -   Code: You can enter API code to edit an image.

            **Examples:** 

            -   `image/resize,w_200`
            -   `image/crop,w_100,h_100/rounded-corners,r_10/format,png`
            **Note:** You can edit images only by using the API of the latest version.

    5.  Click **OK**.

        After the new image style is created, you can apply it to your images through OSS.

-   Apply an image style.
    1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
    2.  Click your bucket name to go to the Overview page of the bucket.
    3.  Choose the **Files** tab, select an existing image or upload a new image to open the Preview page.

        For information about how to upload an image, see [Upload objects](../../../../reseller.en-US/Console User Guide/Upload、download and manage objects/Upload objects.md#).

    4.  Select an image style from the **Image Style** drop-down list.

        You can view the effect of the image that is processed according to the selected style. Additionally, an address that carries the image style is generated accordingly. You can click **Copy File URL** to obtain the URL.


