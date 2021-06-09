---
title: "Creating a Widget"
section: extend
sortOrder: 230
callout: This section is incomplete. Please help to improve it.
---

## Introduction

## Defining a generic widget

## Defining a form widget

### Form widget class

### Form widget properties

### Form widget registration

## Defining a dashboard widget
First Create your Extension 
https://tastyigniter.com/docs/master/extend/extensions

Extension.php
```
    public function extensionMeta()
    {
        return [
            'name'        => 'My Widget Extension',
            'author'      => 'Docs',
            'description' => 'No description provided yet...',
            'icon'        => 'fa-plug',
            'version'     => '1.0.0'
        ];
    }


public function registerDashboardWidgets() {

      return [
          'TastyExt\PluginName\ControllerPath' => [
                'label' => 'Name Of Widget',
                'context' => 'My Content',
            ],
        
        
      ];
    
  }
  ```

### Dashboard widget class
```use Admin\Classes\BaseDashboardWidget;
class ControllerPath extends BaseDashboardWidget
{
  
    /**
     * @var string A unique alias to identify this widget.
     */
    protected $defaultAlias = 'PluginName';

    public function render()
    {
        $this->prepareVars();

        return $this->makePartial('partials/quicklinks');
        /* This would  be your blade file with to display your html */
    }

    public function defineProperties()
    {
        return [
            'title' => [
                'label' => 'Property One',
                'default' => 'test',
                'type' => 'text',
            ]
        ];
    }

    protected function prepareVars()
    {
 
  $this->vars['exampleVar'] = "Some Var"; 
    }

}
```

## Adding Content to Widget 


Inside 
    partials/myblade.blade.php

You can then display any html code you like.

<a href="MyLink"> My Link in my Widget </a>



You can now go to your Dashboard and select your widget from the drop down. 

### Dashboard widget properties

### Dashboard widget registration

## AJAX handlers

## 
