
# WordPress-Useful-Code-Snippets

Customize your site's functionality by use these code snippets.


## Description

These snippets made things easier for both admins and learners, making the website or platform more focused and user-friendly.

# Index

- [Restricting Access to the WordPress Dashboard](#restricting-access-to-the-wordpress-dashboard)
- [Restriction of Access to Theme Functions Without Stopping Specific Function](#restriction-of-access-to-theme-functions-without-stopping-specific-function)
- [Hiding the WordPress Toolbar](#hiding-the-wordpress-toolbar)
- [Disabling Updates Available showing in the WordPress dahsboard plugins menu](#disabling_updates_available_showing_in_the_WordPress_dahsboard_plugins_menu)
- [Usage](#usage)
- [License](#license)

## Restricting Access to the WordPress Dashboard

```
// Restrict access to the dashboard for subscribers

function restrict_dashboard_access() {
    if ( !current_user_can( 'manage_options' ) ) {
        wp_redirect( home_url() );
        exit;
    }
}
add_action( 'admin_init', 'restrict_dashboard_access' );
```
### Restriction of Access to Theme Functions Without Stopping Specific Function

Recently, while completing an LMS site for a client, I utilized the above code snippet in the theme's functions.php file so that only the 'Admin' can see `siteurl.com/wp-admin`. 

However, this snippet somehow conflicted with the LMS plugin I was using. Students were unable to submit their assignments, but the admin could. 

Therefore, I replaced the above code with the following, and it now works perfectly. Students can submit their assignments while they cannot access the site's WordPress dashboard.

```
function restrict_dashboard_access() {
    // Check if the current user is not an administrator and trying to access the dashboard
    if (!current_user_can('manage_options') && strpos($_SERVER['REQUEST_URI'], 'wp-admin') !== false && strpos($_SERVER['REQUEST_URI'], 'admin-ajax.php') === false) {
        // Check if the request is not for LMS Plugin related URLs
        if (strpos($_SERVER['REQUEST_URI'], 'plugin_name') === false) {
            // Redirect the user to the homepage
            wp_redirect(home_url());
            exit;
        }
    }
}
add_action('init', 'restrict_dashboard_access');

```


## Hiding the WordPress Toolbar
For subscribers only - 

```
// Hide the WordPress toolbar for subscribers
function hide_admin_bar_for_subscribers() {
    if (current_user_can('subscriber')) {
        add_filter('show_admin_bar', '__return_false');
    }
}
add_action('after_setup_theme', 'hide_admin_bar_for_subscribers');

```
For all users except admin - 
```
// Hide the WordPress toolbar for all users except admin
function hide_admin_bar_except_admin() {
    if ( !current_user_can('administrator') ) {
        add_filter('show_admin_bar', '__return_false');
    }
}
add_action('after_setup_theme', 'hide_admin_bar_except_admin');

```


## Disabling Updates Available showing in the WordPress dahsboard plugins menu
```
function disable_showing_plugin_updates($value)
{
	if (isset($value) && is_object($value)) {
        // Replace 'plugin-folder/plugin-file.php' with the actual path to the plugin you want to disable updates for
		unset($value->response['plugin-folder/plugin-name.php']); // plugin one
		unset($value->response['plugin-folder/plugin-name.php']); // plugin two
	}
	return $value;
}
add_filter('site_transient_showing_update_plugins', 'disable_plugin_updates');
```

## Usage

**Restricting Access to the WordPress Dashboard:**

This code snippet restricts access to the WordPress dashboard, ensuring that only users with administrative privileges can access and manage the backend settings and content.

Other user roles, such as subscribers or contributors, are redirected away from the dashboard to maintain security and control over site administration.

**Hiding the WordPress Toolbar:**

This code snippet hides the WordPress toolbar, also known as the admin bar, for specific user roles.

By implementing this snippet, users with designated roles, such as subscribers, won't see the toolbar when logged into the site frontend.

**Disabling 'Updates Available' showing in the WordPress dahsboard plugins menu:**

This PHP code snippet disables plugin update notifications for specific plugins in WordPress. It's particularly useful when you want to prevent certain plugins from being updated automatically.


## License

[MIT](https://choosealicense.com/licenses/mit/)
