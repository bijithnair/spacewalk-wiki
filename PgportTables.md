[Tasks Page](PostgresTasks)

*Note*: ~~Tables~~ - are no longer used and do not need to be migrated.
### Tables To Migrate



||*Table*||*Assigned*||*Notes*||||
||~~demo_log.sql~~ || || ||||
||PXTSessions.sql||Vikram ||Done ||
||rhnActionConfigChannel.sql|| Gurjeet || done ||
||rhnActionConfigDateFile.sql|| Gurjeet || done ||
||rhnActionConfigDate.sql|| Gurjeet || done ||
||rhnActionConfigFileName.sql|| Gurjeet || done ||
||rhnActionConfigRevisionResult.sql|| Gurjeet || done ||
||rhnActionConfigRevision.sql|| Gurjeet || done ||
||rhnActionDaemonConfig.sql|| Gurjeet || done ||
||rhnActionErrataUpdate.sql|| Gurjeet || done ||
||rhnActionKickstartFileList.sql|| Gurjeet || done ||
||rhnActionKickstartGuest.sql|| Gurjeet || done ||
||rhnActionKickstart.sql|| Gurjeet || done ||
||rhnActionPackageAnswerfile.sql|| Gurjeet || done ||
||rhnActionPackageDelta.sql|| Gurjeet || done ||
||rhnActionPackageOrder.sql|| Gurjeet || done ||
||rhnActionPackageRemovalFailure.sql|| Gurjeet || done ||
||rhnActionPackage.sql|| Gurjeet || done ||
||rhnActionScript.sql|| Gurjeet || done ||
||rhnAction.sql|| Gurjeet || done ||
||rhnActionStatus.sql|| Gurjeet || done ||
||rhnActionStatus.sql|| Gurjeet || done ||
||rhnActionTransactions.sql|| Gurjeet || done ||
||rhnActionType.sql|| Gurjeet || done ||
||rhnActionVirtDestroy.sql|| Gurjeet || done ||
||rhnActionVirtReboot.sql|| Gurjeet || done ||
||rhnActionVirtRefresh.sql|| Gurjeet || done ||
||rhnActionVirtResume.sql|| Gurjeet || done ||
||rhnActionVirtSchedulePoller.sql|| Gurjeet || done ||
||rhnActionVirtSetMemory.sql|| Gurjeet || done ||
||rhnActionVirtShutdown.sql|| Gurjeet || done ||
||rhnActionVirtStart.sql|| Gurjeet || done ( added FKey (action_id) references rhnAction(id) )||
||rhnActionVirtSuspend.sql|| Gurjeet || done ||
||rhnActionVirtVcpu.sql|| Gurjeet || done ||
||rhnActivationKey.sql||sudhakar ||Done ||
||rhnAllowTrust.sql|| Farrukh || done ||
||rhnAppInstallInstance.sql|| Farrukh || done ||
||rhnAppInstallSessionData.sql|| Farrukh || done ||
||rhnAppInstallSession.sql|| Farrukh || done ||
||rhnArchTypeActions.sql|| Farrukh || done ||
||rhnArchType.sql|| Gurjeet || done ||
||rhnBeehivePathMap.sql||sudhakar ||Done ||
||rhnBlacklistObsoletes.sql||sudhakar ||Done ||
||rhnChannelArch.sql||Vikram ||Done ||
||rhnChannelCloned.sql||sudhakar ||Done ||
||rhnChannelComps.sql||sudhakar ||Done ||
||rhnChannelDownloads.sql||sudhakar ||Done ||
||rhnChannelErrata.sql||sudhakar ||Done ||
||rhnChannelFamilyLicenseConsent.sql||sudhakar ||Done ||
||rhnChannelFamilyLicense.sql||sudhakar ||Done ||
||rhnChannelFamilyMembers.sql||sudhakar ||Done ||
||rhnChannelFamily.sql||Vikram ||Done ||
||rhnChannelFamilyVirtSubLevel.sql||sudhakar ||Done ||
||rhnChannelNewestPackageAudit.sql||sudhakar ||Done ||
||rhnChannelNewestPackage.sql||sudhakar ||Done ||
||rhnChannelPackageArchCompat_data.sql|| || ||contains function in insert stmt.
||rhnChannelPackageArchCompat.sql||sudhakar ||Done ||
||rhnChannelPackage.sql||Vikram ||Done ||
||rhnChannelParent.sql||sudhakar ||Done ||
||rhnChannelPermissionRole.sql||sudhakar ||Done ||
||rhnChannelPermission.sql||sudhakar ||Done ||
||rhnChannelProduct.sql||Vikram ||Done ||
||rhnChannel.sql||Vikram ||Done ||
||rhnChannelTrust.sql||sudhakar ||Done ||
||rhn_check_probe.sql||Vikram ||Done ||
||rhn_check_suite_probe.sql||Vikram ||Done ||
||rhn_check_suites.sql||Vikram ||Done ||
||rhnClientCapabilityName.sql||sudhakar ||Done ||
||rhnClientCapability.sql||sudhakar ||Done ||
||rhn_command_center_state.sql|| Farrukh || Done ||
||rhn_command_class_data.sql|| Farrukh || Done ||
||rhn_command_class.sql|| Farrukh || Done ||
||rhn_command_data.sql|| Farrukh || Done ||
||rhn_command_groups_data.sql|| Farrukh || Done ||
||rhn_command_groups.sql|| Farrukh || Done ||
||rhn_command_parameter_data.sql|| Farrukh || Done ||
||rhn_command_parameter.sql|| Farrukh || Done ||
||rhn_command_param_threshold_data.sql|| Farrukh || Done ||
||rhn_command_param_threshold.sql|| Farrukh || Done ||
||rhn_command_queue_commands_data.sql|| Farrukh || Done ||
||rhn_command_queue_commands.sql|| Farrukh || Done ||
||rhn_command_queue_execs_bk.sql|| Farrukh || Done ||
||rhn_command_queue_execs.sql|| Farrukh || Done ||
||rhn_command_queue_instances_bk.sql|| Farrukh || Done ||
||rhn_command_queue_instances.sql|| Farrukh || Done ||
||rhn_command_queue_params.sql|| Farrukh || Done ||
||rhn_command_queue_sessions.sql|| Farrukh || Done ||
||rhn_command_requirements_data.sql|| Farrukh || Done ||
||rhn_command_requirements.sql|| Farrukh || Done ||
||rhn_command.sql|| Farrukh || Done ||
||rhn_command_target.sql|| Farrukh || Done ||
||rhnConfigChannel.sql|| Gurjeet || done ||
||rhnConfigChannelType.sql||Gurjeet || done ||
||rhnConfigContent.sql||Vikram ||Done ||
||rhnConfigFileFailure.sql|| Gurjeet || done ||
||rhnConfigFile_foreignkeys.sql|| || ||||
||rhnConfigFileName.sql|| Gurjeet || done ||
||rhnConfigFile.sql||Vikram ||Done ||
||rhnConfigFileState.sql||Vikram ||Done ||
||rhnConfigFileType.sql||Vikram ||Done ||
||rhn_config_group_data.sql|| || ||||
||rhn_config_group.sql||Vikram ||Done ||
||rhnConfigInfo.sql||Vikram ||Done ||
||rhn_config_macro_data.sql|| || ||||
||rhn_config_macro.sql||Vikram ||Done ||
||rhn_config_parameter_data.sql|| || ||||
||rhn_config_parameter.sql||Vikram ||Done ||
||rhnConfigRevision.sql||Gurjeet || done ||
||rhn_config_security_type_data.sql|| || ||||
||rhn_config_security_type.sql||Vikram ||Done ||
||rhn_contact_group_members.sql||Vikram ||Done ||
||rhn_contact_groups.sql||Vikram ||Done ||
||rhn_contact_methods.sql||Vikram ||Done ||
||rhnCpuArch_data.sql||sudhakar ||Done ||
||rhnCpuArch.sql||sudhakar ||Done ||
||rhnCpu.sql||sudhakar ||Done ||
||rhnCryptoKeyKickstart.sql||sudhakar ||Done ||
||rhnCryptoKey.sql||sudhakar ||Done ||
||rhnCryptoKeyType.sql||sudhakar ||Done ||
||rhn_current_alerts.sql||Vikram ||Done ||
||rhn_current_state_summaries.sql||Vikram ||Done ||
||rhnCustomDataKey.sql||sudhakar ||Done ||
||rhnCVE.sql||sudhakar ||Done ||
||rhnDaemonState.sql||sudhakar ||Done ||
||rhnDailySummaryQueue.sql||sudhakar ||Done ||
||rhn_db_environment_data.sql|| || ||||
||rhn_db_environment.sql||Vikram ||Done ||
||rhn_deployed_probe.sql||Vikram ||Done ||
||rhnDevice.sql||sudhakar ||Done ||
||rhnDistChannelMap.sql||sudhakar ||Done ||
||rhnDownloads.sql||sudhakar ||Done ||
||rhnDownloadType.sql||sudhakar ||Done ||
||rhnEmailAddress_indexes.sql||sudhakar ||Done ||Merged with rhnEmailAddress.sql.

| rhnEmailAddressLog.sql | sudhakar  | Done  |
| --- | --- | --- |
| rhnEmailAddress.sql | sudhakar  | Done  |
| rhnEmailAddressState_data.sql | sudhakar  | Done  |
| rhnEmailAddressState.sql | sudhakar  | Done  |
| rhnEntitlementLog.sql | sudhakar  | Done  |
| rhn_environment_data.sql | sudhakar  | Done  |
| rhn_environment.sql | Vikram  | Done  |
| rhnErrataBuglist.sql | sudhakar  | Done  |
| rhnErrataBuglistTmp.sql | sudhakar  | Done  |
| rhnErrataCloned.sql | sudhakar  | Done  |
| rhnErrataClonedTmp.sql |  Farrukh  |  Done  |
| rhnErrataCVE.sql |  Farrukh  |  Done  |
| rhnErrataFileChannel.sql |  Farrukh  |  Done  |
| rhnErrataFileChannelTmp.sql |  Farrukh  |  Done  |
| rhnErrataFilePackageSource.sql |  Farrukh  |  Done  |
| rhnErrataFilePackage.sql |  Farrukh  |  Done  |
| rhnErrataFilePackageTmp.sql |  Farrukh  |  Done  |
| rhnErrataFile.sql |  Farrukh  |  Done  |
| rhnErrataFileTmp.sql |  Farrukh  |  Done  |
| rhnErrataFileType.sql |  Farrukh  |  Done  |
| rhnErrataKeyword.sql |  Farrukh  |  Done  |
| rhnErrataKeywordTmp.sql |  Farrukh  |  Done  |
| rhnErrataNotificationQueue.sql |  Farrukh  |  Done  |
| rhnErrataPackage.sql |  Farrukh  |  Done  |
| rhnErrataPackageTmp.sql |  Farrukh  |  Done  |
| rhnErrataQueue.sql |  Farrukh  |  Done  |
| rhnErrataSeverity.sql | Vikram  | Done  |
| rhnErrata.sql | Vikram  | Done  |
| rhnErrataTmp.sql | sudhakar  | Done  |
| rhnErrata_triggers.sql |   |   |  |
| rhnException.sql | Vikram  | Done  |
| ~~ rhnFAQClass.sql ~~ |   |    |  |
| ~~ rhnFAQ.sql ~~  |   |    |  |
| rhnFeature.sql | sudhakar  | Done  |
| rhnFileDownload.sql | sudhakar  | Done  |
| rhnFileListMembers.sql | sudhakar  | Done  |
| rhnFileList.sql | Vikram  | Done  |
| rhnFileLocation.sql | sudhakar  | Done  |
| rhnFile.sql | sudhakar  | Done  |
| rhnGrailComponentChoices.sql | sudhakar  | Done  |
| rhnGrailComponents.sql | sudhakar  | Done  |
| rhn_host_check_suites.sql | Vikram  | Done  |
| rhn_host_probe.sql | Vikram  | Done  |
| rhnIndexerWork.sql | sudhakar  | Done  |
| rhnInfoPane.sql | sudhakar  | Done  |
| rhn_interface_monitoring.sql | Vikram  | Done  |
| rhnKickstartableTree.sql | Vikram  | Done  |
| rhnKickstartChildChannel.sql | sudhakar  | Done  |
| rhnKickstartCommandName.sql | sudhakar  | Done  |
| rhnKickstartCommand.sql | sudhakar  | Done  |
| rhnKickstartDefaultRegToken.sql | sudhakar  | Done  |
| rhnKickstartDefaults.sql | sudhakar  | Done  |
| rhnKickstartIPRange.sql | sudhakar  | Done  |
| rhnKickstartPackage.sql | sudhakar  | Done  |
| rhnKickstartPreserveFileList.sql | sudhakar  | Done  |
| rhnKickstartScript.sql | sudhakar  | Done  |
| rhnKickstartSessionHistory.sql | sudhakar  | Done  |
| rhnKickstartSession.sql | Vikram  | Done  |
| rhnKickstartSessionState_data.sql | sudhakar  | Done  |
| rhnKickstartSessionState.sql | Vikram  | Done  |
| rhnKickstartSession_triggers.sql |   |   |  |
| rhnKickstartTimezone.sql | sudhakar  | Done  |
| rhnKickstartVirtualizationType.sql | Vikram  | Done  |
| rhnKSData.sql | Vikram  | Done  |
| rhnKSInstallType.sql | Vikram  | Done  |
| rhnKSTreeFile.sql | sudhakar  | Done  |
| rhnKSTreeType.sql | Vikram  | Done  |
| rhn_ll_netsaint.sql | Vikram  | Done  |
| rhnMessagePriority.sql | sudhakar  | Done  |
| rhnMessage.sql | sudhakar  | Done  |
| rhnMessageType.sql | sudhakar  | Done  |
| rhn_method_types_data.sql | sudhakar  | Done  |
| rhn_method_types.sql | Vikram  | Done  |
| rhn_metrics_data.sql |   |    |  |
| rhn_metrics.sql | Vikram  | Done  |
| rhnMonitorGranularity.sql | sudhakar  | Done  |
| rhnMonitor.sql | sudhakar  | Done  |
| rhn_multi_scout_threshold.sql | Vikram  | Done  |
| rhn_notification_formats_data.sql |   |   |  |
| rhn_notification_formats.sql | Vikram  | Done  |
| rhn_notifservers.sql | Vikram  | Done  |
| rhnOrgChannelSettings.sql | sudhakar  | Done  |
| rhnOrgChannelSettingsType.sql | sudhakar  | Done  |
| rhnOrgEntitlements.sql | sudhakar  | Done  |
| rhnOrgEntitlementType.sql | sudhakar  | Done  |
| rhnOrgErrataCacheQueue.sql | sudhakar  | Done  |



| rhnOrgInfo.sql | sudhakar  | Done  |
| --- | --- | --- |
| rhnOrgQuota.sql | sudhakar  | Done  |
| rhn_os_commands_xref_data.sql |   |   |  |
| rhn_os_commands_xref.sql | Vikram  | Done  |
| rhn_os_data.sql |   |   |  |
| rhn_os.sql | Vikram  | Done  |
| rhnPackageArch.sql | Vikram  | Done  |
| rhnPackageCapability.sql | Vikram  | Done  |
| rhnPackageChangeLog.sql | sudhakar  | Done  |
| rhnPackageConflicts.sql | sudhakar  | Done  |
| rhnPackageDeltaElement.sql | sudhakar  | Done  |
| rhnPackageDelta.sql | Vikram  | Done  |
| rhnPackageEVR.sql | Vikram  | Done  |
| rhnPackageFileDeleteQueue.sql | sudhakar  | Done  |
| rhnPackageFile.sql | sudhakar  | Done  |
| rhnPackageGroup.sql | Vikram  | Done  |
| rhnPackageKeyAssociation.sql | sudhakar  | Done  |
| rhnPackageKey.sql | sudhakar  | Done  |
| rhnPackageKeyType.sql | sudhakar  | Done  |
| rhnPackageName.sql | Vikram  | Done  |
| rhnPackageNEVRA.sql | sudhakar  | Done  |
| rhnPackageObsoletes.sql | sudhakar  | Done  |
| rhnPackageProvider.sql | sudhakar  | Done  |
| rhnPackageProvides.sql | sudhakar  | Done  |
| rhnPackageRequires.sql | sudhakar  | Done  |
| rhnPackageSenseMap.sql | sudhakar  | Done  |
| rhnPackageSense.sql | sudhakar  | Done  |
| rhnPackageSource.sql | sudhakar  | Done  |
| rhnPackage.sql | Vikram  | Done  |
| rhnPackageSyncBlacklist.sql | sudhakar  | Done  |
| rhn_pager_types_data.sql |   |   |  |
| rhn_pager_types.sql | Vikram  | Done  |
| rhnPaidErrataTempCache.sql | sudhakar  | Done  |
| rhnPathChannelMap.sql | sudhakar  | Done  |
| rhn_physical_location.sql | Vikram  | Done  |
| rhnPrivateChannelFamily.sql | Vikram  | Done  |
| rhn_probe_param_value.sql | Vikram  | Done  |
| rhn_probe.sql | Vikram  |   |  |
| rhn_probe_state.sql | Vikram  | Done  |
| rhn_probe_types_data.sql |   |   |  |
| rhn_probe_types.sql | Vikram  | Done  |
| rhnProductChannel.sql | sudhakar  | Done  |
| rhnProductLine.sql | Sudhakar  | Done  |
| rhnProductName.sql | Vikram  | Done  |
| rhnProduct.sql | Sudhakar  | Done  |
| rhnProvisionState.sql |  Gurjeet  |  done  |
| rhnProxyInfo.sql | Sudhakar  | Done  |
| rhnPublicChannelFamily.sql | Vikram  | Done  |
| rhnPushClient.sql | Sudhakar  | Done  |
| rhnPushClientState_data.sql | Sudhakar  | Done  |
| rhnPushClientState.sql | Sudhakar  | Done  |
| rhnPushDispatcher.sql | Sudhakar  | Done  |
| rhn_quanta_data.sql | Sudhakar  | Done  |
| rhn_quanta.sql | Vikram  | Done  |
| rhnRam.sql | Sudhakar  | Done  |
| rhnRedHatCanonVersion.sql | Sudhakar  | Done  |
| rhn_redirect_criteria.sql | Vikram  | Done  |
| rhn_redirect_email_targets.sql | Vikram  | Done  |
| rhn_redirect_group_targets.sql | Vikram  | Done  |
| rhn_redirect_match_types_data.sql |   |   |  |
| rhn_redirect_match_types.sql | Vikram  | Done  |
| rhn_redirect_method_targets.sql | Vikram  | Done  |
| rhn_redirects.sql | Vikram  | Done  |
| rhn_redirect_types_data.sql |   |   |  |
| rhn_redirect_types.sql | Vikram  | Done  |
| rhnRegTokenChannels.sql |  Farrukh  |  Done  |
| rhnRegTokenConfigChannels.sql |  Farrukh  |  Done  |
| rhnRegTokenEntitlement.sql |  Farrukh  |  Done  |
| rhnRegTokenGroups.sql |  Farrukh  |  Done  |
| rhnRegTokenOrgDefault.sql |  Farrukh  |  Done  |
| rhnRegTokenPackages.sql |  Farrukh  |  Done  |
| rhnRegToken.sql |  Farrukh  |  Done  |
| rhnRelationshipType.sql |  Farrukh  |  Done  |
| rhnReleaseChannelMap.sql |  Farrukh  |  Done  |
| rhn_sat_cluster_probe.sql | Vikram  | Done  |
| rhn_sat_cluster.sql | Vikram  | Done  |
| rhnSatelliteCert.sql | Sudhakar  | Done  |
| rhnSatelliteChannelFamily.sql | Sudhakar  | Done  |
| rhnSatelliteInfo.sql | Sudhakar  | Done  |
| rhnSatelliteServerGroup.sql | Sudhakar  | Done  |
| rhn_satellite_state.sql | Vikram  | Done  |
| rhn_sat_node_probe.sql | Vikram  | Done  |
| rhn_sat_node.sql | Vikram  | Done  |
| rhnSavedSearch.sql | Sudhakar  | Done  |
| rhnSavedSearchType.sql | Sudhakar  | Done  |
| rhn_schedule_days_norm.sql | Vikram  | Done  |
| rhn_schedule_days.sql | Vikram  | Done  |
| rhn_schedules.sql | Vikram  | Done  |
| rhn_schedule_types_data.sql |   |   |  |
| rhn_schedule_types.sql | Vikram  | Done  |
| rhn_schedule_weeks.sql | Vikram  | Done  |
| rhn_semantic_data_type_data.sql | Vikram  | Done  |
| rhn_semantic_data_type.sql |   |   |  |
| rhnServerActionPackageResult.sql | Vikram  | Done  |
| rhnServerActionScriptResult.sql | Vikram  | Done  |
| rhnServerAction.sql |  Gurjeet  |  done  |
| rhnServerActionVerifyMissing.sql | Vikram  | Done  |
| rhnServerActionVerifyResult.sql | Vikram  | Done  |
| rhnServerArch.sql |  Gurjeet  |  done  |
| rhnServerCacheInfo.sql | Vikram  | Done  |
| rhnServerChannelArchCompat_data.sql | Vikram  | Done  |
| rhnServerChannelArchCompat.sql | Vikram  | Done  |
| rhnServerChannel.sql | Vikram  | Done  |
| rhnServerConfigChannel.sql | Vikram  | Done  |
| rhnServerCustomDataValue.sql | Vikram  | Done  |
| rhnServerDMI.sql | Vikram  | Done  |
| rhnServerEvent.sql | Vikram  | Done  |
| rhnServerGroup_indexes.sql | Vikram  | Done  |
| rhnServerGroupMembers_email_triggers.sql | Vikram  | Done  |
| rhnServerGroupMembers_indexes.sql | Vikram  | Done  |
| rhnServerGroupMembers.sql | Vikram  | Done  |
| rhnServerGroupNotes.sql | Vikram  | Done  |
| rhnServerGroup.sql | Vikram  | Done  |
| rhnServerGroupTypeFeature.sql | Vikram  | Done  |
| rhnServerGroupType.sql | Vikram  | Done  |
| rhnServerHistory.sql | Vikram  | Done  |
| rhnServerInfo.sql | Vikram  | Done  |
| rhnServerInstallInfo.sql | Vikram  | Done  |
| rhnServerLocation.sql | Vikram  | Done  |
| rhnServerLock.sql | Vikram  | Done  |
| rhnServerMessage.sql | Vikram  | Done  |
| rhn_server_monitoring_info.sql | Vikram  | Done  |


||rhnServerNeededErrataCache.sql||sudhakar ||Done ||
||rhnServerNeededPackageCache.sql||sudhakar ||Done ||
||rhnServerNetInterface.sql||sudhakar ||Done ||
||rhnServerNetwork.sql||sudhakar ||Done ||
||rhnServerNotes.sql||sudhakar ||Done ||
||rhnServerPackageArchCompat_data.sql||sudhakar ||Done ||
||rhnServerPackageArchCompat.sql||sudhakar ||Done ||
||rhnServerPackage_constraints.sql||sudhakar ||Done ||
||rhnServerPackage_indexes.sql||sudhakar ||Done ||
||rhnServerPackage.sql||sudhakar ||Done ||
||rhnServerPath.sql||sudhakar ||Done ||
||rhnServerPreserveFileList.sql||sudhakar ||Done ||
||rhnServerProfilePackage.sql||sudhakar ||Done ||
||rhnServerProfile.sql||sudhakar ||Done ||
||rhnServerProfileType.sql||sudhakar ||Done ||
||rhnServerServerGroupArchCompat.sql||sudhakar ||Done ||
||rhnServer.sql|| Gurjeet || done ||
||rhnServerTokenRegs.sql||sudhakar ||Done ||
||rhnServerUuid.sql||sudhakar ||Done ||
||rhn_service_probe_origins.sql||sudhakar ||Done ||
||rhnSet.sql||Vikram ||Done ||
||rhnSGTypeBaseAddonCompat.sql||sudhakar ||Done ||
||rhnSGTypeVirtSubLevel.sql||sudhakar ||Done ||
||rhnSnapshotChannel.sql||sudhakar ||Done ||
||rhnSnapshotConfigChannel.sql||sudhakar ||Done ||
||rhnSnapshotConfigRevision.sql||sudhakar ||Done ||
||rhnSnapshotInvalidReason.sql||sudhakar ||Done ||
||rhnSnapshotPackage.sql||sudhakar ||Done ||
||rhnSnapshotServerGroup.sql||sudhakar ||Done ||
||rhnSnapshot.sql||sudhakar ||Done ||
||rhnSnapshotTag.sql||sudhakar ||Done ||
||rhn_snmp_alert.sql||sudhakar ||Done ||
||rhnSNPErrataQueue.sql||sudhakar ||Done ||
||rhnSNPServerQueue.sql||sudhakar ||Done ||
||rhnSolarisPackage.sql||sudhakar ||Done ||
||rhnSolarisPatchedPackage.sql||sudhakar ||Done ||
||rhnSolarisPatchPackages.sql||sudhakar ||Done ||
||rhnSolarisPatchSetMembers.sql||sudhakar ||Done ||
||rhnSolarisPatchSet.sql||sudhakar ||Done ||
||rhnSolarisPatch.sql||sudhakar ||Done ||
||rhnSolarisPatchType.sql||sudhakar ||Done ||
||rhnSourceRPM.sql||Vikram ||Done ||
||rhn_strategies_data.sql||sudhakar ||Done ||
||rhn_strategies.sql||Vikram ||Done ||
||rhnSystemMigrations.sql||Sudhakar ||Done ||
||rhnTagName.sql||Sudhakar ||Done ||
||rhnTag.sql||Sudhakar ||Done ||
||rhnTaskQueue.sql||Sudhakar ||Done ||
||rhnTemplateCategory.sql||Sudhakar ||Done ||
||rhnTemplateString.sql||Sudhakar ||Done ||
||rhnTextMessage.sql||Sudhakar ||Done ||
||rhn_threshold_type_data.sql||Sudhakar ||Done ||
||rhn_threshold_type.sql||Sudhakar ||Done ||
||rhn_time_zone_names_data.sql||Sudhakar ||Done ||
||rhn_time_zone_names.sql||Sudhakar ||Done ||
||rhnTimezone.sql||Sudhakar ||Done ||
||rhnTinyURL.sql||Sudhakar ||Done ||
||rhnTransactionElement.sql||Sudhakar ||Done ||
||rhnTransactionOperation.sql||Sudhakar ||Done ||
||rhnTransactionPackage.sql||Sudhakar ||Done ||
||rhnTransaction.sql||Sudhakar ||Done ||
||rhnTrustedOrgs.sql||Sudhakar ||Done ||
||rhn_units_data.sql||Sudhakar ||Done ||
||rhn_units.sql||Sudhakar ||Done ||
||rhn_url_probe.sql||Sudhakar ||Done ||
||rhn_url_probe_step.sql||Sudhakar ||Done ||
||rhnUserDefaultSystemGroups.sql||Sudhakar ||Done ||
||rhnUserGroup_indexes.sql||Sudhakar ||Done ||
||rhnUserGroupMembers_email_triggers.sql|| || ||||
||rhnUserGroupMembers_indexes.sql||Sudhakar ||Done ||
||rhnUserGroupMembers.sql||Sudhakar ||Done ||
||rhnUserGroup.sql||Sudhakar ||Done ||
||rhnUserGroupType.sql||Sudhakar ||Done ||
||rhnUserInfoPane.sql||Sudhakar ||Done ||
||rhnUserInfo.sql||Sudhakar ||Done ||
||rhnUserInfo_triggers.sql|| || ||||
||rhnUserMessage.sql||Sudhakar ||Done ||
||rhnUserMessageStatus.sql||Sudhakar ||Done ||
||rhnUserMessageType.sql||Sudhakar ||Done ||
||rhnUserReserved.sql||Sudhakar ||Done ||
||rhnUserServerGroupPerms.sql||Sudhakar ||Done ||
||rhnUserServerGroupPerms_triggers.sql|| || ||||
||rhnUserServerPerms.sql||Sudhakar ||Done ||
||rhnUserServerPrefs.sql||Sudhakar ||Done ||
||rhnVersionInfo.sql||Vikram ||Done ||
||rhnVirtSubLevel.sql||Vikram ||Done ||
||rhnVirtualInstanceEventLog.sql||Vikram ||Done ||
||rhnVirtualInstanceEventType.sql||Vikram ||Done ||
||rhnVirtualInstanceInfo.sql||Vikram ||Done ||
||rhnVirtualInstanceInstallLog.sql||Vikram ||Done ||
||rhnVirtualInstance.sql||Vikram ||Done ||
||rhnVirtualInstanceState.sql||Vikram ||Done ||
||rhnVirtualInstanceType.sql||Vikram ||Done ||
|| ~~ rhnVisibleObjects.sql ~~ ||removed || ||||
||rhnWebContactChangeLog.sql||sudhakar ||Done ||
||rhnWebContactChangeState.sql||sudhakar ||Done ||
||rhn_widget_data.sql||sudhakar ||Done ||
||rhn_widget.sql||Vikram ||Done ||
||state_change.sql||sudhakar ||Done ||
||time_series.sql||sudhakar ||Done ||
||valid_countries.sql||sudhakar ||Done ||
||web_contact_indexes.sql|| Gurjeet || done ||
||web_contact.sql|| Gurjeet || done ||
||web_customer_notification.sql|| Farrukh || Done ||
||web_customer.sql|| dgoodwin || done ||
||web_user_contact_permission.sql||sudhakar ||Done ||
||web_user_personal_info.sql|| dgoodwin || ||||
||web_user_prefix.sql|| dgoodwin || ||||
||web_user_site_info.sql||sudhakar ||Done ||
||web_user_site_type.sql||sudhakar ||Done ||