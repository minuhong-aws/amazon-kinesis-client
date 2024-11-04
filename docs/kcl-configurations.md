# KCL Configurations

Here's the table in GitHub README markdown format (.md) for the KCL configuration properties:

| Configuration property | Configuration class | Description | Default value |
|------------------------|----------------------|-------------|---------------|
| applicationName | ConfigsBuilder | The name for this KCL application. Used as the default for the tableName and consumerName. | Not applicable |
| tableName | ConfigsBuilder | Allows overriding the table name used for the Amazon DynamoDB lease table. | Not applicable |
| streamName | ConfigsBuilder | The name of the stream that this application processes records from. | Not applicable |
| workerIdentifier | ConfigsBuilder | A unique identifier that represents this instantiation of the application processor. This must be unique. | Not applicable |
| failoverTimeMillis | LeaseManagementConfig | The number of milliseconds that must pass before you can consider a lease owner to have failed. | 10,000 (10 seconds) |
| shardSyncIntervalMillis | LeaseManagementConfig | The time between shard sync calls. | 60,000 (60 seconds) |
| cleanupLeasesUponShardCompletion | LeaseManagementConfig | When set, leases are removed as soon as the child leases have started processing. | TRUE |
| ignoreUnexpectedChildShards | LeaseManagementConfig | When set, child shards that have an open shard are ignored. This is primarily for DynamoDB Streams. | FALSE |
| maxLeasesForWorker | LeaseManagementConfig | The maximum number of leases a single worker should accept. | Unlimited |
| maxLeaseRenewalThreads | LeaseManagementConfig | Controls the size of the lease renewer thread pool. | 20 |
| billingMode | LeaseManagementConfig | Determines the capacity mode of the lease table created in DynamoDB. | PAY_PER_REQUEST (on-demand mode) |
| initialLeaseTableReadCapacity | LeaseManagementConfig | The DynamoDB read capacity used if KCL needs to create a new DynamoDB lease table with provisioned capacity mode. | 10 |
| initialLeaseTableWriteCapacity | LeaseManagementConfig | The DynamoDB write capacity used if KCL needs to create a new DynamoDB lease table with provisioned capacity mode. | 10 |
| initialPositionInStreamExtended | LeaseManagementConfig | The initial position in the stream that the application should start at. | InitialPositionInStream.TRIM_HORIZON |
| reBalanceThresholdPercentage | LeaseManagementConfig | Percentage value for when to consider reassigning shards among workers. | 10 |
| dampeningPercentage | LeaseManagementConfig | Percentage value to dampen the amount of load moved from overloaded worker in a single rebalance. | 60 |
| allowThroughputOvershoot | LeaseManagementConfig | Determines if additional lease can be taken even if it exceeds desired throughput. | TRUE |
| disableWorkerMetrics | LeaseManagementConfig | Determines if KCL should ignore resource metrics from workers when reassigning leases. | FALSE |
| maxThroughputPerHostKBps | LeaseManagementConfig | Maximum throughput to assign to a worker during lease assignment. | Unlimited |
| isGracefulLeaseHandoffEnabled | LeaseManagementConfig | Controls the behavior of lease handoff between workers. | TRUE |
| gracefulLeaseHandoffTimeoutMillis | LeaseManagementConfig | Minimum time to wait for RecordProcessor to shut down before forcefully transferring lease. | 30,000 (30 seconds) |
| maxRecords | PollingConfig | Maximum number of records that Kinesis returns. | 10,000 |
| retryGetRecordsInSeconds | PollingConfig | Delay between GetRecords attempts for failures. | None |
| maxGetRecordsThreadPool | PollingConfig | Thread pool size used for GetRecords. | None |
| idleTimeBetweenReadsInMillis | PollingConfig | Time KCL waits between GetRecords calls to poll data from streams. | 1,500 |
| callProcessRecordsEvenForEmptyRecordList | ProcessorConfig | When set, record processor is called even for empty record lists. | FALSE |
| parentShardPollIntervalMillis | CoordinatorConfig | How often a record processor should poll for parent shard completion. | 10,000 (10 seconds) |
| skipShardSyncAtWorkerInitializationIfLeaseExist | CoordinatorConfig | Disable shard data sync if lease table contains existing leases. | FALSE |
| shardPrioritization | CoordinatorConfig | Which shard prioritization to use. | NoOpShardPrioritization |
| ClientVersionConfig | CoordinatorConfig | Determines KCL version compatibility mode. | CLIENT_VERSION_CONFIG_3 |
| taskBackoffTimeMillis | LifecycleConfig | Time to wait to retry failed KCL tasks. | 500 (0.5 seconds) |
| logWarningForTaskAfterMillis | LifecycleConfig | Time to wait before logging a warning if a task hasn't completed. | None |
| listShardsBackoffTimeInMillis | RetrievalConfig | Time to wait between calls to ListShards when failures occur. | 1,500 (1.5 seconds) |
| maxListShardsRetryAttempts | RetrievalConfig | Maximum number of times ListShards retries before giving up. | 50 |
| metricsBufferTimeMillis | MetricsConfig | Maximum duration to buffer metrics before publishing to CloudWatch. | 10,000 (10 seconds) |
| metricsMaxQueueSize | MetricsConfig | Maximum number of metrics to buffer before publishing to CloudWatch. | 10,000 |
| metricsLevel | MetricsConfig | Granularity level of CloudWatch metrics to be enabled and published. | MetricsLevel.DETAILED |
| metricsEnabledDimensions | MetricsConfig | Controls allowed dimensions for CloudWatch Metrics. | All dimensions |
