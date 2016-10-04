---
title: HP-GL/2 Caveats
author: windows-driver-content
description: HP-GL/2 Caveats
MS-HAID:
- 'nt5gpd\_3d7b47ed-70bd-4498-8489-0d0ea58197a3.xml'
- 'print.hp\_gl\_2\_caveats'
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 201a894e-5d22-46f8-965d-0e5b88dc54d7
keywords: ["HP-GL/2 monochrome WDK Unidrv , additional considerations", "PCL-5e WDK Unidrv , additional considerations"]
---

# HP-GL/2 Caveats


## <a href="" id="ddk-hp-gl-2-caveats-gg"></a>


1.  HP-GL/2 works only for the version of Unidrv that ships with Windows XP and later operating system releases (Windows XP Unidrv refers to the set of driver files that ship with Windows XP -- unidrv.dll, unidrvui.dll, unires.dll, and stdnames.gpd). It does not work on Windows 2000 Unidrv. If a Windows XP version of Unidrv is present on a machine running Windows 2000 (for example, when a Windows 2000 machine makes a point and print connection to a machine running Windows Server 2003 or later), then the driver uses HP-GL/2.

2.  Some of the rendering commands in the GPD are ignored when HP-GL/2 mode is activated. Instead, hard coded commands in the driver are used. However, those commands must be present in the GPD for the following reasons:
    1.  In later versions of the operating system, the hard coding of rendering commands might be removed.
    2.  An HP-GL/2 driver offers an option to switch to raster mode (that is, to not use the HP-GL/2 driver). For raster mode, all of the commands must be present in the GPD.

        A good rule of thumb is that any PCL-XL/HP-GL/2 command that is used to actually draw something (for example, CmdDownloadPattern or CmdSelectBlackBrush) is ignored. Commands such as Page setup, Document setup, and others that are not drawing commands are not ignored. For more information about these commands, see [Color Commands](color-commands.md).

        Additionally, all HP-GL/2 commands are hard-coded in the driver.

3.  Masks received in calls to [**DrvBitBlt**](https://msdn.microsoft.com/library/windows/hardware/ff556180) and other bit-block transfer functions may not work correctly.

4.  When Windows XP Unidrv is used on Windows 2000 and HP-GL/2 is activated, some graphics rendering functions may not work correctly. For example, the output from [**DrvGradientFill**](https://msdn.microsoft.com/library/windows/hardware/ff556236) calls has red and blue reversed.

5.  Unidrv assumes that printer hardware supports ROP commands. If a printer does not support ROP, some documents might not print correctly.

6.  Support for hatch brushes is required. If the printer does not support hatch brushes, then the output depends on how the printer hardware handles the hatch brush selection command (FT21,x SV21,x).

7.  The color of a hatch brush is ignored for monochrome printers. It is always printed as black.

8.  For color printers, HP-GL/2 supports only 24 bpp/600 dpi. For monochrome printers, HP-GL/2 supports only 600 dpi. If your printer supports other values, constrain HP-GL/2 mode to be chosen only when color depth is 24 bpp and resolution is 600 dpi. The following example shows how the GraphicsMode feature can be modified to accomplish this. In this example, the first \*Constraints entry causes Unidrv to reject a mode change to HPGL2MODE if the Resolution feature's Option2 value is not 600x600 dpi. (In the example, it is assumed that the Option2 value is some lower resolution, such as 300x300 dpi.) The second \*Constraints entry causes Unidrv to reject the mode change if the ColorMode feature's options are Color or 8bpp.
    ```
    *Feature: GraphicsMode
    {
      *rcNameID: =GRAPHICSMODE_DISPLAY
      *FeatureType: DOC_PROPERTY
      *HelpIndex: 12000
      *DefaultOption: HPGL2MODE
      *Option: HPGL2MODE
       {
         *rcNameID: =GRAPHICSMODE_HPGL2_DISPLAY
         *Constraints: Resolution.Option2
         *Constraints: LIST(ColorMode.Color, ColorMode.8bpp)
       }
      *Option: RASTERMODE
       {
         *rcNameID: =GRAPHICSMODE_RASTER_DISPLAY
       }
    }
    ```

9.  Color printers must be able to scale images on the hardware. This requirement does not exist for monochrome printers.

10. For monochrome printers, it is assumed that:
    -   The printer accepts only 1bpp information.
    -   A bit set to 1 indicates a black pixel, and a bit set to 0 indicates a white pixel.
    -   The printer cannot gray scale any color. (This arises naturally from the 1 bpp limitation.)

11. The following compression methods must be supported:
    -   No compression
    -   TIFF
    -   Delta Row

12. HP-GL/2 does not perform system landscape rotation. When HP-GL/2 is enabled, the printer is assumed to handle the rotation of rasters, fonts, and coordinates for pages printed in landscape mode. To counter this problem, make sure that all GPD rotation parameters (the \*RotateCoordinate?, \*RotateFont?, and \*RotateRaster? attributes) are set to **TRUE**. If your printer has memory overflow problems with rotation, you should consider not activating HP-GL/2, or placing constraints on memory (that is, HP-GL/2 should be activated only if memory is 4 MB or more.

    On low-memory devices (for example, a 600 dpi monochrome laser printer with 2 MB of RAM), certain pages that generate out-of-memory errors when the device is in HP-GL/2 mode might print correctly in raster mode. One solution for devices with less than a full bitmap of memory is to write the GPD so that raster mode is the default, and to let the system handle landscape rotation, rather than HP-GL/2. In addition, certain complex portrait print jobs might print correctly in raster mode, but not in HP-GL/2 mode. If that is the case, you should consider making raster mode the default.

13. Print optimization functionality on the **Advanced** tab of the printer property pages is currently ignored in HP-GL/2 mode.

14. \*MirrorRasterPage? is not supported in HP-GL/2 mode.

15. It is possible for TrueType outline fonts to be downloaded as raster fonts even when the GPD file specifies that the device supports outline fonts. This can occur for a variety of reasons (for example, insufficient memory on the printer).

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bprint\print%5D:%20HP-GL/2%20Caveats%20%20RELEASE:%20%289/1/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")

