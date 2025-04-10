IJobWorkerBuilderStep1 NewWorker();

IActivateJobsCommandStep1 NewActivateJobsCommand();

IUpdateRetriesCommandStep1 NewUpdateRetriesCommand(long jobKey);

IDeployResourceCommandStep1 NewDeployCommand();

IEvaluateDecisionCommandStep1 NewEvaluateDecisionCommand();

ICreateProcessInstanceCommandStep1 NewCreateProcessInstanceCommand();

ICancelProcessInstanceCommandStep1 NewCancelInstanceCommand(long processInstanceKey);

ISetVariablesCommandStep1 NewSetVariablesCommand(long elementInstanceKey);

IResolveIncidentCommandStep1 NewResolveIncidentCommand(long incidentKey);

IPublishMessageCommandStep1 NewPublishMessageCommand();

ITopologyRequestStep1 TopologyRequest();