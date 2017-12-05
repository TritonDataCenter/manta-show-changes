#
# This Source Code Form is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, You can obtain one at http://mozilla.org/MPL/2.0/.
#

#
# Copyright (c) 2017, Joyent, Inc.
#

#
# IMPORTANT: This Makefile should consist solely of repo-specific configuration,
# plus "include" directives for the common Makefile components, plus trivial,
# repo-specific targets.  Repo-specific targets and recipes are okay, but
# generic targets and recipes do NOT belong here.  If you find yourself wanting
# to add support for new targets here, you should add them to a new or existing
# pluggable Makefile component, document it clearly, and include that Makefile
# here.
#


NPM =			npm
JSON_FILES =		package.json
JS_FILES =		bin/manta-show-changes
JSL_FILES_NODE =	$(JS_FILES)
JSSTYLE_FILES =		$(JS_FILES)
JSL_CONF_NODE =		tools/jsl.node.conf

include ./tools/mk/Makefile.defs
include ./tools/mk/Makefile.node_modules.defs

#
# Repo-specific targets
#
.PHONY: all
all: $(STAMP_NODE_MODULES)

include ./tools/mk/Makefile.node_modules.targ
include ./tools/mk/Makefile.targ
