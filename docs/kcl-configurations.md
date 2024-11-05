# KCL Configurations

You can set configuration properties to customize Kinesis Client Library's functionality to meet your specific requirements. The following table describes configuration properties and classes.

> [!Important]
> In KCL 3.x, the load balancing algorithm aims to achieve even CPU utilization across workers, not an equal number of leases per worker. Setting `maxLeasesForWorker` too low, you might limit KCL's ability to balance the workload effectively. If you use the `maxLeasesForWorker` configuration, consider increasing its value to allow for the best possible load distribution.

## List of KCL configuration properties
| Configuration property | Configuration class | Description | Default value |
|---|---|---|---:|
| applicationName | ConfigsBuilder | The name for this the KCL application. Used as the default for the tableName and consumerName. | N/A |
| tableName | ConfigsBuilder | Allows overriding the table name used for the Amazon DynamoDB lease table. | N/A |
| streamName | ConfigsBuilder | The name of the stream that this application processes records from. | N/A |
| workerIdentifier | ConfigsBuilder | A unique identifier that represents this instantiation of the application processor. This must be unique. | N/A |
| failoverTimeMillis | LeaseManagementConfig | The number of milliseconds that must pass before you can consider a lease owner to have failed. For applications that have a large number of shards, this may be set to a higher number to reduce the number of DynamoDB IOPS required for tracking leases. | 10000 (10 seconds) |
| shardSyncIntervalMillis | LeaseManagementConfig | The time between shard sync calls. | 60000 (60 seconds) |
| cleanupLeasesUponShardCompletion | LeaseManagementConfig | When set, leases are removed as soon as the child leases have started processing. | TRUE |
| ignoreUnexpectedChildShards | LeaseManagementConfig | When set, child shards that have an open shard are ignored. This is primarily for DynamoDB Streams. | FALSE |
| maxLeasesForWorker | LeaseManagementConfig | The maximum number of leases a single worker should accept. Setting it too low may cause data loss if workers can't process all shards, and lead to a suboptimal lease assignment among workers. Consider total shard count, number of workers, and worker processing capacity when configuring it. | Unlimited |
| maxLeaseRenewalThreads | LeaseManagementConfig | Controls the size of the lease renewer thread pool. The more leases that your application could take, the larger this pool should be. | 20 |
| billingMode | LeaseManagementConfig | Determines the capacity mode of the lease table created in DynamoDB. There are two options: on-demand mode (PAY_PER_REQUEST) and provisioned mode. We recommend using the default setting of on-demand mode because it automatically scales to accommodate your workload without the need for capacity planning. | PAY_PER_REQUEST (on-demand mode) |
| initialLeaseTableReadCapacity | LeaseManagementConfig | The DynamoDB read capacity that is used if the Kinesis Client Library needs to create a new DynamoDB lease table with provisioned capacity mode. You can ignore this configuration if you are using the default on-demand capacity mode in billingMode configuration. | 10 |
| initialLeaseTableWriteCapacity | LeaseManagementConfig | The DynamoDB read capaciy that is used if the Kinesis Client Library needs to create a new DynamoDB lease table. You can ignore this configuration if you are using the default on-demand capacity mode in billingMode configuration. | 10 |
| initialPositionInStreamExtended | LeaseManagementConfig | The initial position in the stream that the application should start at. This is only used during initial lease creation. | InitialPositionInStream.TRIM_HORIZON |
| reBalanceThresholdPercentage | LeaseManagementConfig | A percentage value that determines when the load balancing algorithm should consider reassigning shards among workers.  This is a new configuration introduced in KCL 3.x. | 10 |
| dampeningPercentage | LeaseManagementConfig | A percentage value that is used to dampen the amount of load that will be moved from the overloaded worker in a single rebalance operation.  This is a new configuration introduced in KCL 3.x. | 60 |
| allowThroughputOvershoot | LeaseManagementConfig | Determines whether additional lease still needs to be taken from the overloaded worker even if it causes total amount of lease throughput taken to exceed the desired throughput amount.  This is a new configuration introduced in KCL 3.x. | TRUE |
| disableWorkerMetrics | LeaseManagementConfig | Determines if KCL should ignore resource metrics from workers (such as CPU utilization) when reassigning leases and load balancing. Set this to TRUE if you want to prevent KCL from load balancing based on CPU utilization.  This is a new configuration introduced in KCL 3.x. | FALSE |
| maxThroughputPerHostKBps | LeaseManagementConfig | Amount of the maximum throughput to assign to a worker during the lease assignment.  This is a new configuration introduced in KCL 3.x. | Unlimited |
| isGracefulLeaseHandoffEnabled | LeaseManagementConfig | Controls the behavior of lease handoff between workers. When set to true, KCL will attempt to gracefully transfer leases by allowing the shard's RecordProcessor sufficient time to complete processing before handing off the lease to another worker. This can help ensure data integrity and smooth transitions but may increase handoff time. When set to false, the lease will be handed off immediately without waiting for the RecordProcessor to shut down gracefully. This can lead to faster handoffs but may risk incomplete processing.  Note: Checkpointing must be implemented inside the shutdownRequested() method of the RecordProcessor to get benefited from the graceful lease handoff feature.  This is a new configuration introduced in KCL 3.x. | TRUE |
| gracefulLeaseHandoffTimeoutMillis | LeaseManagementConfig | Specifies the minimum time (in milliseconds) to wait for the current shard's RecordProcessor to gracefully shut down before forcefully transferring the lease to the next owner.   If your processRecords method typically runs longer than the default value, consider increasing this setting. This ensures the RecordProcessor has sufficient time to complete its processing before the lease transfer occurs.  This is a new configuration introduced in KCL 3.x. | 30000 (30 seconds) |
| maxRecords | PollingConfig | Allows setting the maximum number of records that Kinesis returns. | 10000 |
| retryGetRecordsInSeconds | PollingConfig | Configures the delay between GetRecords attempts for failures. | none |
| maxGetRecordsThreadPool | PollingConfig | The thread pool size used for GetRecords. | none |
| idleTimeBetweenReadsInMillis | PollingConfig | Determines how long KCL waits between GetRecords calls to poll the data from data streams. The unit is milliseconds. | 1500 |
| callProcessRecordsEvenForEmptyRecordList | ProcessorConfig | When set, the record processor is called even when no records were provided from Kinesis. | FALSE |
| parentShardPollIntervalMillis | CoordinatorConfig | How often a record processor should poll to see if the parent shard has been completed. The unit is milliseconds. | 10000 (10 seconds) |
| skipShardSyncAtWorkerInitializationIfLeasesExist | CoordinatorConfig | Disable synchronizing shard data if the lease table contains existing leases. | FALSE |
| shardPrioritization | CoordinatorConfig | Which shard prioritization to use. | NoOpShardPrioritization |
| ClientVersionConfig | CoordinatorConfig | Determines which KCL version compatibility mode the application will run in. This configuration is only for the migration from previous KCL versions. When migrating to 3.x, you need to set this configuration to CLIENT_VERSION_CONFIG_COMPATIBLE_WITH_2x. You can remove this configuration when you complete the migration. | CLIENT_VERSION_CONFIG_3X |
| taskBackoffTimeMillis | LifecycleConfig | The time to wait to retry failed KCL tasks. The unit is milliseconds. | 500 (0.5 seconds) |
| logWarningForTaskAfterMillis | LifecycleConfig | How long to wait before a warning is logged if a task hasn't completed. | none |
| listShardsBackoffTimeInMillis | RetrievalConfig | The number of milliseconds to wait between calls to ListShards when failures occur. The unit is milliseconds. | 1500 (1.5 seconds) |
| maxListShardsRetryAttempts | RetrievalConfig | The maximum number of times that ListShards retries before giving up. | 50 |
| metricsBufferTimeMillis | MetricsConfig | Specifies the maximum duration (in milliseconds) to buffer metrics before publishing them to CloudWatch. | 10000 (10 seconds) |
| metricsMaxQueueSize | MetricsConfig | Specifies the maximum number of metrics to buffer before publishing to CloudWatch. | 10000 |
| metricsLevel | MetricsConfig | Specifies the granularity level of CloudWatch metrics to be enabled and published.   Possible values: NONE, SUMMARY, DETAILED. | MetricsLevel.DETAILED |
| metricsEnabledDimensions | MetricsConfig | Controls allowed dimensions for CloudWatch Metrics. | All dimensions |

## New configurations in KCL 3.x
The following configuration properties are newly added in KCL 3.x: 
| Configuration property | Configuration class | Description | Default value |
|---|---|---|---:|
| reBalanceThresholdPercentage | LeaseManagementConfig | A percentage value that determines when the load balancing algorithm should consider reassigning shards among workers.  This is a new configuration introduced in KCL 3.x. | 10 |
| dampeningPercentage | LeaseManagementConfig | A percentage value that is used to dampen the amount of load that will be moved from the overloaded worker in a single rebalance operation.  This is a new configuration introduced in KCL 3.x. | 60 |
| allowThroughputOvershoot | LeaseManagementConfig | Determines whether additional lease still needs to be taken from the overloaded worker even if it causes total amount of lease throughput taken to exceed the desired throughput amount.  This is a new configuration introduced in KCL 3.x. | TRUE |
| disableWorkerMetrics | LeaseManagementConfig | Determines if KCL should ignore resource metrics from workers (such as CPU utilization) when reassigning leases and load balancing. Set this to TRUE if you want to prevent KCL from load balancing based on CPU utilization.  This is a new configuration introduced in KCL 3.x. | FALSE |
| maxThroughputPerHostKBps | LeaseManagementConfig | Amount of the maximum throughput to assign to a worker during the lease assignment.  This is a new configuration introduced in KCL 3.x. | Unlimited |
| isGracefulLeaseHandoffEnabled | LeaseManagementConfig | Controls the behavior of lease handoff between workers. When set to true, KCL will attempt to gracefully transfer leases by allowing the shard's RecordProcessor sufficient time to complete processing before handing off the lease to another worker. This can help ensure data integrity and smooth transitions but may increase handoff time. When set to false, the lease will be handed off immediately without waiting for the RecordProcessor to shut down gracefully. This can lead to faster handoffs but may risk incomplete processing.  Note: Checkpointing must be implemented inside the shutdownRequested() method of the RecordProcessor to get benefited from the graceful lease handoff feature.  This is a new configuration introduced in KCL 3.x. | TRUE |
| gracefulLeaseHandoffTimeoutMillis | LeaseManagementConfig | Specifies the minimum time (in milliseconds) to wait for the current shard's RecordProcessor to gracefully shut down before forcefully transferring the lease to the next owner.   If your processRecords method typically runs longer than the default value, consider increasing this setting. This ensures the RecordProcessor has sufficient time to complete its processing before the lease transfer occurs.  This is a new configuration introduced in KCL 3.x. | 30000 (30 seconds) |


## Discontinued configuration properties in KCL 3.x
The following configuration properties are discontinued in KCL 3.x:
| Configuration property | Configuration class | Description |
|---|---|---|
| maxLeasesToStealAtOneTime | LeaseManagementConfig | The maximum number of leases an application should attempt to steal at one time. KCL 3.x will ignore this configuration and reassign leases based on the resource utilization of workers. |
| enablePriorityLeaseAssignment | LeaseManagementConfig | Controls whether workers should prioritize taking very expired leases (leases not renewed for 3x the failover time) and new shard leases, regardless of target lease counts but still respecting max lease limits. KCL 3.x will ignore this configuration and always spread expired leases across workers. |

> [!Important]
> You still must have the discontinued configuration properties during the migration from previous KCL verisons to KCL 3.x. During the migration, KCL workers will first start with the KCL 2.x compatible mode and switch to the KCL 3.x functionality mode when it detects that all KCL workers of the application are ready to run KCL 3.x. These discontinued configurations are needed while KCL workers are running the KCL 2.x compatible mode.