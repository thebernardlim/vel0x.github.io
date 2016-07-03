---

layout: single
title: Xcode Build Tokens
date: 2014-09-18 14:44:24
comments: disqus
tags: [xcode, build, programming, os x]

---

Recently, I was working with a larger iOS project than I usually do. This 
project involved creating a library and using it in other applications as well
as testing it. In order to make this as painless as possible I started using
bash script to make my builds as efficient as possible. In doing this
I discovered many many Xcode build tokens which are underused. Lots of these
made my build scripts a lot easier to create as I didn't need to work with lots
of relative paths, and helped ignore lots of redundant builds and build steps. 

One of the biggest issues that I had with these tokens was the lack of
documentation by Apple on these. A good chunk of these are based on my
observations of how they change with various project settings. Some of them may
not be quite right and if that is the case, feel free to let me know on this one
and I'll be glad to update it. I'm taking the unusual step on this post and
adding in a comment system to see how well received this is (as well as test the
        suitablity of a comment system for this site), so please let me know
through there.

Anyway, here are some of the more useful tokens that I found:

<ul>

<li><b>BUILD_DIR:</b> The location of the build directory. This is normally somewhere deep in the derived data folder, but can be configured in Xcode.<br><ul><li><code>/some/project/path/build/build</code></li></ul></li>
<li><b>BUILD_ROOT:</b> This doesn't seem to be mentioned anywhere in Apple's documentation(just like lots of flags here), but seems to be set to the same thing as BUILD_DIR.<br><ul><li><code>/some/project/path/build/build</code></li></ul></li>
<li><b>BUILT_PRODUCTS_DIR:</b> The location where the final products are placed after building. <br><ul><li><code>/some/project/path/build/build/Debug-iphoneos</code></li></ul></li>
<li><b>COMPRESS_PNG_FILES:</b> A boolean value which specifies whether to compress PNG files whenthey are copied into the application bundle on iOS.<br><ul><li><code>YES</code></li></ul></li>
<li><b>CONFIGURATION:</b> The current build configuration. This is usually Debug or Release,but can be a custom configuration.<br><ul><li><code>Debug</code></li></ul></li>
<li><b>CONFIGURATION_BUILD_DIR:</b> The build directory for the current configuration. This seems togenerally be the same as the BUILD_PRODUCTS_DIR.<br><ul><li><code>/some/project/path/build/build/Debug-iphoneos</code></li></ul></li>
<li><b>CURRENT_ARCH:</b> Identifies the architecture on which the build is being *performed*.<br><ul><li><code>i386</code></li></ul></li>
<li><b>DEBUGGING_SYMBOLS:</b> A boolean which specifies if debug symbols should be included in thecurrent build.<br><ul><li><code>YES</code></li></ul></li>
<li><b>DEVELOPMENT_LANGUAGE:</b> Specifies the language that the project was developed in. Note thatthis means English/Spanish/etc. and *not* Objective-C/C++/etc.<br><ul><li><code>English</code></li></ul></li>
<li><b>EFFECTIVE_PLATFORM_NAME:</b> The platform which the build is being performed for, prefixed with a- usually. This can be combined with CONFIGURATION to get builddirectories usually.<br><ul><li><code>-iphoneos</code></li></ul></li>
<li><b>INSTALL_OWNER:</b> The user who owns the final built product. This can be used to getappropriate permissions, etc.<br><ul><li><code>SomeUser</code></li></ul></li>
<li><b>IPHONEOS_DEPLOYMENT_TARGET:</b> This one is simply the iOS deployment target. If you are developingfor iOS 7.0 then set it to 7.0.<br><ul><li><code>7.0</code></li></ul></li>
<li><b>ONLY_ACTIVE_ARCH:</b> A boolean flag which states if the build will be performed for theactive architecture only.<br><ul><li><code>YES</code></li></ul></li>
<li><b>OPTIMIZATION_LEVEL:</b> The optimisation level which is used by GCC compatible compilers.<br><ul><li><code>0</code></li></ul></li>
<li><b>PATH:</b> The PATH variable which is the same as you would see in a regularBash environment.<br><ul><li><code>"/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/usr/local/bin:/usr/bin:/bin:/usr/sbin"</code></li></ul></li>
<li><b>PLATFORM_NAME:</b> The name of the current platform. This time there is no - unlike theEFFECTIVE_PLATFORM_NAME.<br><ul><li><code>iphoneos</code></li></ul></li>
<li><b>PRODUCT_NAME:</b> The current products name. This is usually the same name as the currentbuild target.<br><ul><li><code>"Universal Build"</code></li></ul></li>
<li><b>PROJECT:</b> The name of the project.<br><ul><li><code>SuperAwesomeProject</code></li></ul></li>
<li><b>PROJECT_DIR:</b> The location of the .xcodeproj directory.<br><ul><li><code>/some/project/path/</code></li></ul></li>
<li><b>PROJECT_FILE_PATH:</b> Similar to PROJECT_DIR, but this one places the .xcodeproj directoryon the end.<br><ul><li><code>/some/project/path/build/Redstone_Common.xcodeproj</code></li></ul></li>
<li><b>PROJECT_NAME:</b> Seems to be identical to PROJECT.<br><ul><li><code>SuperAwesomeProject</code></li></ul></li>
<li><b>SDK_NAME:</b> The current SDK being used.<br><ul><li><code>iphoneos7.1</code></li></ul></li>
<li><b>SOURCE_ROOT:</b> The location of the source code for this project.<br><ul><li><code>/some/project/path</code></li></ul></li>
<li><b>SRCROOT:</b> An alias for SOURCE_ROOT<br><ul><li><code>/some/project/path</code></li></ul></li>
<li><b>TARGETNAME:</b> The name of the current target.<br><ul><li><code>"Universal Build"</code></li></ul></li>
<li><b>TARGET_BUILD_DIR:</b> The build directory of the current target.<br><ul><li><code>/some/project/path/build/build/Debug-iphoneos</code></li></ul></li>
<li><b>TARGET_NAME:</b> An alias for TARGETNAME.<br><ul><li><code>"Universal Build"</code></li></ul></li>
<li><b>USER:</b> The current system user.<br><ul><li><code>SomeUser</code></li></ul></li>

</ul>
