#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Customize Styling

The default QAFE application uses some styling like the color scheme and background images, which can be customized fully to accomodate your particular layout.

In order to do so the main folders to look for are `src/main/webapp/css` and `src/main/webapp/images` from within your project. The main QAFE css style sheet exists in the css folder and is named `qafe.less`. The file extension `less` exposes its interior format: it's basic CSS combined with more powerful extension provided by `Less` (see http://lesscss.org/).

The style sheet `qafe.less` is the starting point for altering the complete look and feel. Though the images can be found in the aforementioned location, they are being referred to in this style sheet.

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
