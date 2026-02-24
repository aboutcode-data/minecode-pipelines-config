minecode-pipelines configuration
==================================

This is released at pypi: https://pypi.org/project/minecode-pipelines/

To install `minecode-pipelines` with scancode.io:

* Clone https://github.com/aboutcode-org/scancode.io
* Specify the federatedcode settings in ``.env`` file
* Run ``make clean && make dev-mining && make run``
* Then select and start the mining pipeline according to which ecosystem
  you want to mine Package-URLs (PURLs) from.

Configuration format
=======================

* configuration/checkpoints for each ecossytem would be stored in a root folder
  with the same name as the package type defined in https://github.com/package-url/purl-spec (example: ``pypi``)

* ``checkpoints.json`` stores checkpoints related to the PURL mining like:

  * last serial number processed (used in indexes at pypi, npm etc)
  * last processed commit (where the data is stored in git repos)
  * directory to store las fetched index data
    (like the JSON fetched from pypi simple with package names and last updated info)
  * state information in ``state``:

    * ``null``: mining has not started.
    * ``initital-sync`` : at the start of mining we need to mine a huge
      amount of packages for PURL to catch up.
      This is typically very large and could take several hours to several days
      dependening on the ecosystem size.
      We fetch and save an index state and mine all PURLs till there.
      Once we reach a state where remaining
      new PURLs can be mined in a couple hours, we can move on to
      the next state where we mine new PURLs
      added in a periodic manner.
    * ``periodic-sync`` : This is a periodic update of new PURLs
      added in the index in a period, and typically this
      should not take more than a couple hours.

  * optional elements to improve readability/debugging:

    * ``last_updated``: date and time of last checkpoint update

* ``packages_checkpoints.json``: stores checkpoint related to:

  * ``packages_mined``: which packages have been mined in the ``initital-sync`` state.
