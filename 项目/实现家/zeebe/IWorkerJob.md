string JOB_TYPE { get; }
string WORK_NAME { get; }
int WORK_TYPE { get; }
Task BusinessHandler(IJobClient jobClient, IJob job);