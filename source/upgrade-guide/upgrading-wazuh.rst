.. Copyright (C) 2021 Wazuh, Inc.

.. _upgrading_wazuh_server:

Upgrading the Wazuh manager
===========================

This section describes how to upgrade the Wazuh manager to the latest available version, which includes upgrading to the latest compatible version of Open Distro for Elasticsearch or Elastic Stack basic licence. 

.. note::
  In order to reduce server downtime, it is recommended to update the master node first

.. note:: Root user privileges are required to execute all the commands described below.


#. Add the Wazuh repository:


    .. tabs::


      .. group-tab:: Yum


        .. include:: ../_templates/installations/wazuh/yum/add_repository.rst



      .. group-tab:: APT


        .. include:: ../_templates/installations/wazuh/deb/add_repository.rst



      .. group-tab:: ZYpp


        .. include:: ../_templates/installations/wazuh/zypp/add_repository.rst    


#. Stop the Wazuh manager:

    .. tabs::

 
      .. group-tab:: Systemd


        .. code-block:: console

          # systemctl stop wazuh-manager


      .. group-tab:: SysV Init

        .. code-block:: console

          # service wazuh-manager stop


#. Upgrade the Wazuh manager to the latest version:


    .. tabs::


      .. group-tab:: Yum

         .. code-block:: console

            # yum upgrade wazuh-manager



      .. group-tab:: APT


          .. code-block:: console

              # apt-get install wazuh-manager



      .. group-tab:: ZYpp


          .. code-block:: console

              # zypper update wazuh-manager
    

#. Restart the Wazuh manager:
    
   .. include:: ../_templates/installations/wazuh/common/enable_wazuh_manager_service.rst





             













.. note::
  The configuration file of the Wazuh manager will not be replaced in the updates if it has been modified, so the settings of the new capabilities will have to be added manually. More information can be found at the :ref:`User manual <user_manual>`.

  If Wazuh runs in a multi-node cluster, it is necessary to update all Wazuh managers to the same version. Otherwise, Wazuh nodes will not join the cluster.

Disabling the Wazuh repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is recommended to disable the Wazuh repository in order to avoid undesired upgrades and compatibility issues:

.. tabs::

  .. group-tab:: Yum

    .. code-block:: console

      # sed -i "s/^enabled=1/enabled=0/" /etc/yum.repos.d/wazuh.repo

  .. group-tab:: APT

    This step is not necessary if the user set the packages to a ``hold`` state instead of disabling the repository.

    .. code-block:: console

      # sed -i "s/^deb/#deb/" /etc/apt/sources.list.d/wazuh.list
      # apt-get update

    Alternatively, the user can set the package state to ``hold``, which will stop updates. It will be still possible to upgrade it manually using ``apt-get install``:

    .. code-block:: console

      # echo "wazuh-manager hold" | sudo dpkg --set-selections

  .. group-tab:: ZYpp

    .. code-block:: console

      # sed -i "s/^enabled=1/enabled=0/" /etc/zypp/repos.d/wazuh.repo

Next step
---------

:ref:`Upgrading Elasticsearch, Kibana and Filebeat<upgrade_elasticsearch_filebeat_kibana>`.
