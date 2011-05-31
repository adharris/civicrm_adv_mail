# CiviCRM Advanced Mail #

This is a drupal6 module which tweaks the way CiviMail handles unsubscriptions.  The main change is a seperation of which groups to send mail to and which groups to include in the mailing.  This is accomplished by requiring the user to select at least one "Unsubscription Group" from which contact unsubscription data will be used.

 * Unsubscription Groups are all "mailing groups," private or public.
 * "Include/Exclude" groups for mailing can now be used to select all groups, mailing or not
 * If a mailing group is included in the mailing, that group must also be included as an unsubscription group
 * If a contact is unsubcribed from one or more of the specified mailing groups, that contact will not be included in the mailing.
   * If a user is not a member of one or more unsubscription groups, they will still be included int the mailing (_Unsubscription groups are only used to determine who should not receive the email_)
 * When a contact unsubscribes from a mailing, they will be removed from *all* of the unsubscription groups.
   * If a user is not currently a member of one of the unsubscription groups, they will be added to the group with status "Removed"

## Example Use Case ##

The conceptual use case is that the CiviCRM Mailing Groups will be mailing topics or mailing lists.  Therefore a CiviCRM user can send an email to selection of normal groups without allowing the contacts to unsubscribe from those normal groups.  Instead, the unsubscription is tracked else where.

Suppose, for example, we have a smart group called "VIP Doners", consisting of all contacts who donated more thatn $500 dollars in the past year.  If we want to send the "VIP Doners" group a special solicitation in vanilla CiviMail, we would do the following:

 * Change the "*VIP Doners*" group into a mailing list
 * Create a new CiviMail Mailing and include the "*VIP Doners*" group.

This has some problems.  Firstly, if a contact unsubscribes from the mailing sent via this method, they will be removed from the *VIP Doners* group, which means future attempts to identify large doners will omit that contact if the group is used.  Once the group is exposed as a mailing list, _the meaning of the group changes_, in our case membership in the "*VIP Doners*" group now means "Donated more than $500 in the past year _and_ has not unsubscribed from this group."  This renders the group useless for everything that is not a mailing.

The second problem is that most large organizations will already have a "Fundraising Newsletter" mailing group.  There is a good change that some of our VIP doners have already unsubscribed from this group, and therefore will be suprised/irked that they are recieving another solicitation.  Since the contact has unsubscribed from the fundraising _topic_, they should be unsubscribed from our targeted fundraising solicitation also.

With this module, both these issues are addressed, and the workflow becomes:

 * Create a new mailing, select "*Fundraising Newsletter*" as unsubscrition group, select "*VIP Doners*" as an included group
 * Email will go to all VIP doners, except those who have previously unsubscribed from the Fundraising newsletter.

## Installation ##

This module requires CiviCRM 3.4.2 and Drupal 6.  Install the module as any drupal module would be install.  Then, copy or symlink "Group.tpl" to [Your CiviCRM Custom Template Directory](http://wiki.civicrm.org/confuence/display/CRMDOC40/Directories)/CRM/Mailing/Form/Group.tpl
 
For CiviCRM 3.4.0 and 3.4.1, you can apply the following patches and the module _should_ work:
 * [CRM-8207](http://issues.civicrm.org/jira/browse/CRM-8207)
 * [CRM-8190](http://issues.civicrm.org/jira/browse/CRM-8207)
 * [CRM-8171](http://issues.civicrm.org/jira/browse/CRM-8207)
 * [CRM-8142](http://issues.civicrm.org/jira/browse/CRM-8207)
 * [CRM-8114](http://issues.civicrm.org/jira/browse/CRM-8207)
