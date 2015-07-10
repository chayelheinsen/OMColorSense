# Forked from [OMColorSense](https://github.com/omz/ColorSense-for-Xcode)
This fork enables color sense for [UIColor colorWithHex:@"ff00ad"];
* Currently, OMColorSense will only sense hex codes of 6 or 8 in length. ie ff00ad (pink) and ff00adff (a light blue)
Apple does not provide this method but you can implement it yourself:

```objc
+ (UIColor *)colorWithHex:(NSString *)hexString {
    // We can accept a hex string with or without #
    NSString *colorString = [[hexString stringByReplacingOccurrencesOfString:@"#" withString:@""] uppercaseString];
    
    CGFloat alpha = 0.0, red = 0.0, blue = 0.0, green = 0.0;
    
    switch (colorString.length) {
            
        case 3: // #RGB
            
            alpha = 1.0f;
            red   = [self colorComponentFrom:colorString start:0 length:1];
            green = [self colorComponentFrom:colorString start:1 length:1];
            blue  = [self colorComponentFrom:colorString start:2 length:1];
            break;
            
        case 4: // #ARGB
            
            alpha = [self colorComponentFrom:colorString start:0 length:1];
            red   = [self colorComponentFrom:colorString start:1 length:1];
            green = [self colorComponentFrom:colorString start:2 length:1];
            blue  = [self colorComponentFrom:colorString start:3 length:1];
            break;
            
        case 6: // #RRGGBB
            
            alpha = 1.0f;
            red   = [self colorComponentFrom:colorString start:0 length:2];
            green = [self colorComponentFrom:colorString start:2 length:2];
            blue  = [self colorComponentFrom:colorString start:4 length:2];
            break;
            
        case 8: // #AARRGGBB
            
            alpha = [self colorComponentFrom:colorString start:0 length:2];
            red   = [self colorComponentFrom:colorString start:2 length:2];
            green = [self colorComponentFrom:colorString start:4 length:2];
            blue  = [self colorComponentFrom:colorString start:6 length:2];
            break;
            
        default:
            NSLog(@"%@ is not valid hex", hexString);
            break;
    }
    
    return [UIColor colorWithRed:red green:green blue:blue alpha:alpha];
}

+ (CGFloat)colorComponentFrom:(NSString *)string start:(NSUInteger)start length:(NSUInteger)length {
    NSString *substring = [string substringWithRange: NSMakeRange(start, length)];
    NSString *fullHex = length == 2 ? substring : [NSString stringWithFormat: @"%@%@", substring, substring];
    
    unsigned hexComponent;
    
    [[NSScanner scannerWithString: fullHex] scanHexInt: &hexComponent];
    
    return hexComponent / 255.0;
}

```

# ColorSense for Xcode

## Overview

ColorSense is an Xcode plugin that makes working with `UIColor` (and `NSColor`) more visual.

There are [many](http://www.colorchooserapp.com) [tools](http://iconfactory.com/software/xscope) that allow you to insert a `UIColor`/`NSColor` from a color picker or by picking a color from the screen. But once you've inserted it, it can be hard to remember which color you're actually looking at in your code because you basically just have a series of numbers.

This is where ColorSense comes in: When you put the caret on one of your colors, it automatically shows the actual color as an overlay, and you can even adjust it on-the-fly with the standard Mac OS X color picker.

The plugin also adds some items to the _Edit_ menu to insert colors and to disable color highlighting temporarily. These menu items have no keyboard shortcuts by default, but you can set them via the system's keyboard preferences (Xcode's own preferences won't show them).

**[Watch Demo Video (YouTube)](http://www.youtube.com/watch?v=eblRfDQM0Go)**

<a href="http://flattr.com/thing/879121/omzColorSense-for-Xcode-on-GitHub" target="_blank">
<img src="http://api.flattr.com/button/flattr-badge-large.png" alt="Flattr this" title="Flattr this" border="0" /></a>

## Installation

Simply build the Xcode project and restart Xcode. The plugin will automatically be installed in `~/Library/Application Support/Developer/Shared/Xcode/Plug-ins`. To uninstall, just remove the plugin from there (and restart Xcode).

If you get a "Permission Denied" error while building, please see [this issue](https://github.com/omz/ColorSense-for-Xcode/issues/1).

This is tested on OS X 10.8 with Xcode 4.4.1 and 4.5.

## Limitations

* It only works for constant colors, something like `[UIColor colorWithWhite:foo * bar + 1 alpha:baz]` won't work.

* Only RGB (`colorWithRed:green:blue:alpha:`), grayscale (`colorWithWhite:alpha:`), and named colors (`redColor`...) are supported at the moment (no HSB or CMYK).

## License

    Copyright (c) 2012, Ole Zorn
    All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions are met:

    * Redistributions of source code must retain the above copyright notice, this
      list of conditions and the following disclaimer.

    * Redistributions in binary form must reproduce the above copyright notice,
      this list of conditions and the following disclaimer in the documentation
      and/or other materials provided with the distribution.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
    ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
    WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
    DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
    FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
    DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
    OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
    CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
    OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
    OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
