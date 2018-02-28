# wordpress-boilerplate

## Getting started

Following the steps below will leave you with a fresh Wordpress install, using the [Halland theme](https://github.com/regionhalland/halland). 

**1. Make sure the following dependencies are installed on your computer:**
- [Virtualbox](https://www.virtualbox.org/wiki/Downloads) >= 4.3.10
- [Vagrant](https://www.vagrantup.com/intro/getting-started/index.html) >= 1.8.5

**2. Create a folder with the project name:**

```
$ mkdir myproject && cd myproject
```

**3. Clone this repo in your directory and remove the .git folder:**

```
git clone --depth=1 git@github.com:RegionHalland/wordpress-boilerplate.git && rm -rf .git
```

**4. Initialize a new GitHub repos for your new site using the [GitHub Desktop client](https://desktop.github.com/).**

**5. Update the following files, replacing `example.com` with your local site name, like `myproject.test` and adding your [ACF-key](#acf) under `ACF_PRO_KEY`**
```
- ./trellis/group_vars/development/wordpress_sites.yml
- ./trellis/group_vars/development/vault.yml
```

For more options, see [Trellis documentation](https://roots.io/trellis/docs/wordpress-sites/#options).

**6. (Optional) Configure the IP address at the top of the `./trellis/vagrant.default.yml` to allow for multiple vagrant boxes to be run concurrently (default is 192.168.50.5).**

**7. 🚨Are you going to run a multisite?** 

Follow the steps under [”Multisite”](#multisite).

**8. Provision your server from the `./trellis` directory:**

```
$ cd trellis && vagrant up
```

## <a name="acf"></a>Setting up Advanced Custom Fields

TBD!

## Multisite

It’s important to follow these steps before provisioning your server.

**1. Update multisite settings in `./trellis/group_vars/development/wordpress_sites`:**
```
multisite:
  enabled: true
  subdomains: false
```

**2. Add the following code somewhere in `./site/config/application.php`:**
```
/* Multisite */
define('WP_ALLOW_MULTISITE', true);
define('MULTISITE', true);
define('SUBDOMAIN_INSTALL', false); // Set to true if using subdomains
define('DOMAIN_CURRENT_SITE', env('DOMAIN_CURRENT_SITE'));
define('PATH_CURRENT_SITE', env('PATH_CURRENT_SITE') ?: '/');
define('SITE_ID_CURRENT_SITE', env('SITE_ID_CURRENT_SITE') ?: 1);
define('BLOG_ID_CURRENT_SITE', env('BLOG_ID_CURRENT_SITE') ?: 1);
```

**3. Install the multisite-url-fixer mu-plugin:**
```
$ cd site && composer require roots/multisite-url-fixer
```

**4. Provision your server from the `./trellis` directory:**
```
$ cd trellis && vagrant up --provision
```
