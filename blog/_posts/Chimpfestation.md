---
layout: post
published: false
featured: false
---

## Your Basic Monkey

A few weeks ago, we released Mailchimp Module version 7.x-3.0-beta1 on Drupal.org. The third major revision of the Mailchimp Module for Drupal 7 is actually the 5th major revision of the module, including 2 versions for Drupal 6. ThinkShout Partner Lev Tsypin rolled the first release in January of 2008, and the first version of the project page included a little about his goals for the module:
> Right now, I am focusing on two types of integration:

> 1. Using hook_user to maintain a members list in MailChimp.
> 2. Having an opt in field in the user profile which uses one of the MC merge fields to allow for segmenting the members into those who want to receive communications.
> 3. Having an anonymous sign up form to enroll users in a general newsletter.

The module (and the project page!) have both come a long way since then, but the functionality described in that initial post has remained the core of the module through each version: _Anonymous signup forms_ and _Authenticated subscription control_ describe the core use cases that have resulted in over 15,000 installs. Sure, there's Campaign integration, Activity reporting, and all sorts of bells and whistles around list and subscription management, but Anonymous signup forms and User-based subscription control are the bread and butter.

## Identity Crisis

Building on the success of the Mailchimp module, ThinkShout has made the contribution of robust, useful Drupal modules a core part of our business. In building [Entity Registration](https://drupal.org/project/registration), [RedHen](https://drupal.org/project/redhen), [Salesforce v3](https://drupal.org/project/salesforce), [Leaflet](https://drupal.org/project/leaflet), and a bunch of other great modules, we've often leveraged Drupal 7's Entity and Field systems to make our tools as versatile and abstract as possible, to allow for any imaginable use-case.

We had a bit of a wake-up call when one of our favorite clients, [The Salmon Project](http://www.salmonlove.com/), asked us to integrate their fancy new RedHen CRM directly with Mailchimp. Integrating RedHen Contact Entities doesn't actually match up with either of these: _Anonymous signup forms_ and _Authenticated subscription control_.

It was time to bring ThinkShout's signature versatility and abstraction to ThinkShout's signature module.

## Monkeys everywhere!

The first thing we did was to de-couple the configuration of Anonymous Signup Forms and Authenticated Subscription Control. The Mailchimp Lists configuration UI had grown into a bit of a monster: it included 16 separate options, not counting merge field sync settings, ranging from the "**Submit button label** on the signup form to the **Roles allowed** to access this list on User configuration pages. Rather than framing everything around each list, we broke things out by their Drupal-side functionality:

1. The Signup Module was created for generating anonymous List Signup forms.
2. The List Module now provide a Field type: "Mailchimp Subscription", which leverages Field UI to allow any Entity to become an independently-controlled Mailchimp List Subscriber.

What does this mean? If all you need to do is generate some anonymous subscription blocks or pages, the Mailchimp Signup module has you covered. Just enable it, go to the "Signup Forms" tab in the Mailchimp Admin UI, and create a signup! The UI lets you: generate blocks or pages easily; include one or more lists on each form; pick which Merge Fields to include; and voila!
![signup_ui.png](/assets/images/blog/signup_ui.png)

If, however, you want to subscribe some type of Entity to a Mailchimp List (like a User, say, or a RedHen Contact), you can now do that lickity-split using Field UI:
![field_type.png](/assets/images/blog/field_type.png)
This handy Mailchimp Signup field will insist on being tied to one of your Mailchimp Lists. Once that's done, you can configure instances of this Field like you would any other Mailchimp field. It will automatically pull in the available Merge Fields, and let you select which Properties or Fields from the Entity you want to push into these fields:
![field_instance_config.png](/assets/images/blog/field_instance_config.png)
Want to default your Entity to Subscribed or Unsubscribed? Use Field UI's built-in configuration options. Use field display options to hide the field if you want to, or display it as a form right on the Entity.

Do you want to get the old User-role-based subscription behavior? Easily done with a simple Rule or two and some basic field configuration! We've included the custom rules actions you need, and there's even an example rule in the Readme file for mailchimp_lists.

What this all boils down to is: do what you want! You can Mailchimp-ify any Entity on your site with an email address in under 5 minutes. So go ape!

## Pealing Away Campaign Complexity

New ThinkShouter Dan Ruscoe brought huge improvement to the Campaign module, including the ability to send to list segments from directly within Drupal and some awesome UI improvements. We have long offered the ability to pull site content into campaigns, but you had to come up with the exact token for the content on your own: not the simplest task, especially if you have a non-developer creating your campaigns.

Now? A simple drop-down interface. Create a view mode for your entity types specifically for use in Campaigns, or re-use an existing view mode. Just select your content type, the view mode, and search by Title, and the module generates the token for you. Pop it into your Campaign anywhere you want.
![site_content_embed_ui.png](/assets/images/blog/site_content_embed_ui.png)

He also added a handy mergefield key selector patterned after the Token UI.
![merge_vars_ui.png](/assets/images/blog/merge_vars_ui.png)

## Other Evolutions

We didn't stop with fancy configuration options. Heck, we didn't _start_ with fancy configuration options. The goofs at Mailchimp HQ released the 2.0 version of their API, and we wouldn't want you using that archaic 1.x nonsense, so we re-wrote the entire core of the Mailchimp Module to leverage the new API. While we were at it, we re-wrote our asyncronous functionality to make it much simpler and less error-prone. It might not be easy enough for a monkey to understand at this point, but it's certainly more tolerant of a little monkeying.