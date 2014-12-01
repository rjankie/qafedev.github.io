#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Customize Styling

The default QAFE application uses some styling like the color scheme and background images, which can be customized fully to accomodate your particular layouting needs.

The main QAFE CSS style sheet is named `qafe.less`. When building your QAFE project after starting a new QAFE project this file is copied from QAFE platform's folder `${HOME}/qafe/qafe-web-gwt-[x].[y]/css/qafe.less` to `target/[project-name]/css/`. 
Or to be more general: any file in `${HOME}/qafe/qafe-web-gwt-[x].[y]` is copied to `target/[project-name]`.

The location of `qafe.less` in the Project Explorer of eclipse:

![qafeless](https://github.com/qafedev/qafedev.github.io/blob/master/assets/images/eclipse-qafe-less.png)

Note that the file extension `less` exposes its interior format: it's basic CSS combined with more powerful extension provided by `Less` (see http://lesscss.org/).

The style sheet `qafe.less` is the starting point for altering the complete look and feel. Though the images can be found in the aforementioned location, they are being referred to in this style sheet.

Before adapting the style sheet make sure you're using your own copy by copying the file to `src/main/webapp` of your project. That way the QAFE platform version of `qafe.less` (or any other static file) remains the blueprint for any QAFE project you start. Your project's copy of `qafe.less` gets preference over the one from QAFE platform.

For example, the background images that can be found in the Hello World window are defined in the style sheet like this:
```css
.qafe_body .gwt-DecoratedPopupPanel .popupMiddleCenter {
    background-color: #fff;
    background:
       url('css/assets/img/bg-body-leaves.png') left bottom no-repeat,
       url('css/assets/img/bg-logo.png') right bottom no-repeat,
       url('css/assets/img/bg-body-grad.png') left bottom no-repeat,
       linear-gradient(to bottom, #ffffff 0%,#eaeaea 100%) left bottom no-repeat;
}
```

When your own corporate images are in place simply adapting the background URLs like `'css/assets/img/bg-logo.png'` can be used to accomdate the styling to your particular needs.
