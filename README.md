
# WordPress-role-customization

Customize user roles and permissions in WordPress to restrict dashboard access and hide the toolbar for specific user roles.




## Description

As I was building an LMS site for my client, I found these code snippets to be invaluable.

The first one helped me restrict access to the WordPress dashboard, ensuring that only admins could manage the site's settings and content.

The second snippet was equally useful as it allowed me to hide the WordPress toolbar, creating a cleaner interface for learners without distracting admin features.

These snippets simplified the user experience for both administrators and learners, making the LMS platform more focused and user-friendly.
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
## Usage

**Restricting Access to the WordPress Dashboard:**

This code snippet restricts access to the WordPress dashboard, ensuring that only users with administrative privileges can access and manage the backend settings and content.

Other user roles, such as subscribers or contributors, are redirected away from the dashboard to maintain security and control over site administration.

**Hiding the WordPress Toolbar:**

This code snippet hides the WordPress toolbar, also known as the admin bar, for specific user roles.

By implementing this snippet, users with designated roles, such as subscribers, won't see the toolbar when logged into the site frontend.


## License

[MIT](https://choosealicense.com/licenses/mit/)
