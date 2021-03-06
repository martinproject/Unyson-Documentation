Sidebars
========

A way to customize WordPress pages with dynamic sidebars.

.. contents::
    :local:
    :backlinks: top

Configuration
-------------

.. code-block:: php

    <?php if (!defined('FW')) die('Forbidden');

    // file: framework-customizations/extensions/sidebars/config.php

    $cfg = array(
        'sidebar_positions' => array(
            'position-id' => array(
                /**
                 * Image from: framework-customizations/extensions/sidebars/images/
                 * (required)
                 */
                'icon_url' => 'picture.png',
                /**
                 * Number of sidebars on page.
                 * The maximum number is 4.
                 * (optional)
                 * (default 0)
                 */
                'sidebars_number' => 0
            ),
            // other positions ...
        ),
        /**
         * Array that will be passed to register_sidebar($args)
         * Should be without 'id' and 'name'.
         * Will be used for all dynamic sidebars.
         */
        'dynamic_sidebar_args' => array(
            'before_widget' => '<div id="%1$s" class="widget %2$s">',
            'after_widget'  => '</div>',
            'before_title'  => '<h3>',
            'after_title'   => '</h3>',
        ),
    );

Helpers
-------

* ``fw_ext_sidebars_show($color)`` - display sidebar in frontend. The parameter ``$color`` is the color of the sidebar selected from the WordPress admin and can be: ``blue``, ``yellow``, ``green`` or ``red``.
* ``fw_ext_sidebars_get_current_position()`` - can be called in the frontend to find out current position name. It returns ``position-id`` from ``$cfg['sidebar_positions']``, or null.
* ``fw_ext_sidebars_get_current_preset()`` - can be called in the frontend to find out the sidebar's settings for current page template.

    .. code-block:: php

        // file: sidebar-content.php

        <?php if (!defined('FW')) die('Forbidden');

        $current_position = fw_ext_sidebars_current_position_get();

        if ($current_position !== 'position-id') {
            echo fw_ext_sidebars_show('green');
        }

        if ($current_position === 'position-id-2') {
            echo fw_ext_sidebars_show('blue');
        }

        if ($current_position === 'position-id-3') {
            echo fw_ext_sidebars_show('yellow');
        }

Filters
-------

* ``fw_ext_sidebars_post_types`` - use this filter to change/remove post types that are used in the extension.

    .. code-block:: php

        function _filter_remove_post_type_from_sidebars($post_types_list) {
            unset($post_types_list['post_type_name']);

            return $post_types_list;
        }
        add_filter('fw_ext_sidebars_get_post_types', '_filter_remove_post_type_from_sidebars' );

* ``fw_ext_sidebars_taxonomies`` - use this filter to change/remove taxonomies that are used in the extension.

    .. code-block:: php

        function _filter_remove_taxonomy_from_sidebars($taxonomy_list) {
            unset($taxonomy_list['taxonomy_name']);

            return $taxonomy_list;
        }
        add_filter('fw_ext_sidebars_get_taxonomies', '_filter_remove_taxonomy_from_sidebars');
