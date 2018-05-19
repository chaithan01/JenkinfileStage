import org.jenkinsci.plugins.workflow.graph.FlowNode
import org.jenkinsci.plugins.workflow.cps.nodes.StepStartNode
import org.jenkinsci.plugins.workflow.actions.LabelAction
import hudson.model.Action;


pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

def isLabelAction(FlowNode flowNode){
    def actions = flowNode.getActions()
    for (Action action: actions){
        if (action instanceof LabelAction) {
            return true
        }
    }
    return false
}

def getStartStepNode(List<FlowNode> flowNodes){
    def currentFlowNode = null
    def labelAction = null

    for (FlowNode flowNode: flowNodes){
        selectedFlowNode = flowNode
        labelAction = false
        if (flowNode instanceof StepStartNode){
            labelAction = isLabelAction(flowNode)
        }
        if (labelAction){
            return flowNode
        }
    }
    if (selectedFlowNode == null) {
        return null
    }
    return getStartStepNode(selectedFlowNode.getParents())
}

def getBuildStage(build){
    def execution = build.getExecution()
    def currentExecutionHeads = execution.getCurrentHeads()
    def startStepNode = getStartStepNode(currentExecutionHeads)
    if(startStepNode){
        return startStepNode.getDisplayName()
    }
}

def printlnStageSample(){
    for (job in Hudson.instance.getAllItems(org.jenkinsci.plugins.workflow.job.WorkflowJob)) { 
        for (build in job.builds) {
             println getBuildStage(build)
    }
}



