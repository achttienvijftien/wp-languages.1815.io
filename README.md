# wp-languages.1815.io

## Features
- Automatically creates composer packages for WordPress translations from api.wordpress.org.
- This repo provides custom satis repository for WordPress languages. See more in [wp-languages.1815.io](https://wp-languages.1815.io/).
- To add more language files please submit a pull request to the `src` branch.
- Repos are updated every half hour.

## Example configuration with composer

This example adds all translations from finnish and french packages.
```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://wp-languages.1815.io",
            "only": [
                "koodimonni-language/*",
                "koodimonni-plugin-language/*",
                "koodimonni-theme-language/*"
            ]
        }
    ],
    "require": {
        "koodimonni-language/fi": "*",
        "koodimonni-language/fr_fr": "*"
    },
    "extra": {
        "dropin-paths": {
            "web/app/languages/": ["vendor:koodimonni-language"],
            "web/app/languages/plugins/": ["vendor:koodimonni-plugin-language"],
            "web/app/languages/themes/": ["vendor:koodimonni-theme-language"]
        },
    }
}
```

## Request packages
If package what you're looking for can't be found look first the response from api:

[https://api.wordpress.org/translations/core/1.0/](https://api.wordpress.org/translations/core/1.0/)

If your language is not listed in the response we can't help you. You'll need to ask from wp-core translators instead.

This is pretty static repository, and we can't add all possible plugins here.

## Manually adding any language zip to your composer.json
There's also manual method of including any translation in WordPress.org. This is useful because we can't include all plugins in this repository. Let's look how to add french (fr_FR) jetpack translations. First search the api for jetpack translations:

```bash
$ curl -s 'https://api.wordpress.org/translations/plugins/1.0/?slug=jetpack' | python -m json.tool
```

Choose your language zip from json output for example for us it was:
https://downloads.wordpress.org/translation/plugin/jetpack/3.9.2/fr_FR.zip

Then add it to your composer like this example. You just need to update the version number everytime the package updates.
```json
{
    "repositories": [
        {
            "type": "package",
            "package": {
                "name": "koodimonni-plugin-language/jetpack-fr_fr",
                "type": "wordpress-language",
                "version": "3.9.2",
                "dist": {
                    "type": "zip",
                    "url": "https://downloads.wordpress.org/translation/plugin/jetpack/3.9.2/fr_FR.zip",
                    "reference": "master"
                }
            }
        }
    ],
    "require": {
        "koodimonni/composer-dropin-installer": "*",
        "koodimonni-plugin-language/jetpack-fr_fr": ">=3.9.2"
    },
    "extra": {
        "dropin-paths": {
            "dropin-paths": {
            "web/app/languages/": ["vendor:koodimonni-language"],
            "web/app/languages/plugins/": ["vendor:koodimonni-plugin-language"],
            "web/app/languages/themes/": ["vendor:koodimonni-theme-language"]
        },
        }
    }
}
```

## License

This project is licensed under the MIT License - see the [LICENSE][] file for details

[LICENSE]: https://github.com/achttienvijftien/wp-languages.1815.io/blob/src/LICENSE