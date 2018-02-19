# Steps for Titanium RTL Application:

1 - Add `android:supportsRtl="true"` to <application> tag in _tiapp.xml_
```xml
<android xmlns:android="http://schemas.android.com/apk/res/android">
    <manifest>
        <application android:supportsRtl="true" />
    </manifest>
</android>
```
2 - Use if in .tss file to switch RTL & LTR
```javascript
// alloy.js
Alloy.Globals.isRTL = Ti.App.Properties.hasProperty('language') ? true : false;
```
```css
'#exampleLabel' : {
	color: ‘red'
}
'#exampleLabel[if=Alloy.Globals.isRTL]' : {
	right: 5
}
'#exampleLabel[if=!Alloy.Globals.isRTL]' : {
	left: 5
}
```
3 - add toggle button to change between languages then reopen the index to see the changes
```javascript
//index.js
var button = Ti.UI.createButton({title: "Change Language"});

button.addEventListener("click", function() {
	
	if (Ti.Locale.currentLanguage == 'ar') {
	
		Ti.Locale.setLanguage('en');
		Ti.App.Properties.removeProperty('language');
		Alloy.Globals.isRTL = false;
		
	} else {
	
		Ti.Locale.setLanguage('ar');
		Ti.App.Properties.setString('language', 'ar');
		Alloy.Globals.isRTL = true;
		
	}
	
	Alloy.createController("index").getView().open();
});
```

4 - Set iOS 9+ is minimum in tiapp.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ti:app xmlns:ti="http://ti.appcelerator.org">
  <ios>
    <min-ios-ver>9.0</min-ios-ver>
  </ios>
</ti:app>
```
5 - Set Android 4.2 as minimum
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ti:app xmlns:ti="http://ti.appcelerator.org">
  <android xmlns:android="http://schemas.android.com/apk/res/android">
    <manifest android:versionCode="17">
      <uses-sdk android:minSdkVersion="17"/>
    </manifest>
  </android>
</ti:app>
```

# This items flip from/to RTL/LTR fine
## iOS 9+
1. LeftNavButton & RightNavButton
2. Default ListView/TableView templates
3. Window open transition come from right with RTL and left with LTR
4. CollectionView list items from top right with RTL and top left with LTR

## Android 4.2+
1. ActionBar and menus items flip RTL/LTR with zero code
2. Drawer layout come from right instead of left


# Current Titanium issues RTL support issues
1. linearlayout (horizontal/vertical layout) will not flip, Titanium calculate positions manually, same with autolayout in iOS. [ref](http://android-developers.blogspot.com.eg/2013/03/native-rtl-support-in-android-42.html)
2. Natural direction for the text supported nativly in both iOS and Android, while Titanium has only right, left and center allignment. [Android](https://developer.android.com/reference/android/view/View.html#attr_android:textDirection) [iOS](https://developer.apple.com/reference/uikit/uilabel/1620541-textalignment)
3. Titanium has no API to change local, but we can use this [module](http://shareourideas.com/2013/12/02/titanium-locale-module-for-both-android-and-ios/)
4. Titanium has an issue to re-open the hole UI stack on iOS, so change loal will load strings from the new selected file but will not wich RTL/LTR till you close the app and open it again.
