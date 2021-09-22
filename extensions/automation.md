---
title: "Automation Extension"
section: "extensions"
sortOrder: 20
---

## Introduction

Automation within TastyIgniter fire when other specific actions have taken place. Rules are run through every time an order state is changed and triggered if the changes match the conditions defined in the rule.

An action is attached to the automation, such as send mail to customer or send print jobs to printer.

## Installation

To install this extension, click on the **Add to Site** button on the TastyIgniter marketplace item page or search for **Igniter.Automation** in **Admin System > Updates > Browse Extensions**

## Admin Panel

Automations are managed in the admin panel by navigating to **Tools > Automations**.

## Automation workflow

When an automation is triggered, it uses the following workflow:

- Extension registers associated actions, conditions and events using `registerAutomationRules`
- When a system event is fired `Event::fire`, the parameters of the event are captured, along with any global parameters
- A command is pushed on the queue to process the automation `Queue::push`
- The command finds all automation rules using the event class and triggers them
- The automation conditions are checked and only proceed if met
- The automation actions are triggered

## Usage

**Example of Registering Automation Rules**

The `presets` definition specifies automation rules defined by the system.

```
public function registerAutomationRules()
{
    return [
        'events' => [
            \Igniter\User\AutomationRules\Events\CustomerRegistered::class,
        ],
        'actions' => [
            \Igniter\User\AutomationRules\Actions\SendMailTemplate::class,
        ],
        'conditions' => [
            \Igniter\User\AutomationRules\Conditions\CustomerAttribute::class
        ],
        'presets' => [
            'registration_email' => [
                'name' => 'Send customer registration email',
                'event' => \Igniter\User\AutomationRules\Events\CustomerRegistered::class,
                'actions' => [
                    \Igniter\User\AutomationRules\Actions\SendMailTemplate::class => [
                        'template' => 'igniter.user::mail.registration_email'
                    ],
                ]
            ]
        ],
    ];
}
```

**Example of Registering Global Parameters**
These parameters are available globally to all automation rules.

```
\Igniter\Automation\Classes\EventManager::instance()->registerCallback(function($manager) {
    $manager->registerGlobalParams([
        'customer' => Auth::customer()
    ]);
});
```

**Example of an Event Class**

An event class is responsible for preparing the parameters passed to the conditions and actions.

```
class CustomerRegisteredEvent extends \Igniter\Automation\Classes\BaseEvent
{
    /**
     * Returns information about this event, including name and description.
     */
    public function eventDetails()
    {
        return [
            'name' => 'Registered',
            'description' => 'When a customer registers',
            'group' => 'customer'
        ];
    }

    public static function makeParamsFromEvent(array $args, $eventName = null)
    {
        return [
            'user' => array_get($args, 0)
        ];
    }
}
```

**Example of an Action Class**

Action classes define the final step in an automation and perform the automation itself.

```
class SendMailTemplate extends \Igniter\Automation\Classes\BaseAction
{
    /**
     * Returns information about this action, including name and description.
     */
    public function actionDetails()
    {
        return [
            'name' => 'Compose a mail message',
            'description' => 'Send a message to a recipient',
        ];
    }

    /**
     * Field configuration for the action.
     */
    public function defineFormFields()
    {
        return [
            'fields' => [],
        ];
    }

    /**
     * Triggers this action.
     * @param array $params
     * @return void
     */
    public function triggerAction($params)
    {
        $email = 'test@email.tld';
        $template = $this->model->template;

        Mail::sendTo($email, $template, $params);
    }
}
```

**Example of a Condition Class**

A condition class must declare an `isTrue` method for evaluating whether the condition is true or not.

```
class MyCondition extends \Igniter\Automation\Classes\BaseCondition
{
    /**
     * Returns information about this condition, including name and description.
     */
    public function conditionDetails()
    {
        return [
            'name' => 'Condition',
            'description' => 'My Condition is checked',
        ];
    }

    /**
     * Checks whether the condition is TRUE for specified parameters
     * @param array $params
     * @return bool
     */
    public function isTrue(&$params)
    {
        return true;
    }
}
```

**Example of a Model Attribute Condition Class**

A condition class applies conditions to sets of model attributes.

```
class CustomerAttribute extends \Igniter\Automation\Classes\BaseCondition
{
    /**
     * Returns information about this condition, including name and description.
     */
    public function conditionDetails()
    {
        return [
            'name' => 'Customer attribute',
        ];
    }
    
    public function defineModelAttributes()
    {
        return [
            'first_name' => [
                'label' => 'First Name',
            ],
            'last_name' => [
                'label' => 'Last Name',
            ],
        ];
    }

    /**
     * Checks whether the condition is TRUE for specified parameters
     * @param array $params
     * @return bool
     */
    public function isTrue(&$params)
    {
        return true;
    }
}
```
