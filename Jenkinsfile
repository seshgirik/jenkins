node("master") {
  println "welcome to parallel project"   
  def staticTests = [: ]
  staticTests["codeAnalysis"] = {
    stage("staticAnalysis") {}
  }
  staticTests["codeInspection"] = {
    stage("Steps to cotnnue or abort") {
        
        input 'Continue to deploy stage?'

    }
  }

  stage("codeAnalysis") {
    println staticTests
    //{codeAnalysis=org.jenkinsci.plugins.workflow.cps.CpsClosure2@2bd63470, codeInspection=org.jenkinsci.plugins.workflow.cps.CpsClosure2@2088ecae}

    parallel(staticTests)

  }

  builds = allBuilds()

  for (stages in builds) {
    parallel(stages)
  }

}
def allBuilds() {
  buildsarray = []
  //for (i = 0; i < 4; i++) {
    for (os in ["Rhel", "centos", "windows"]) {
    stages = [: ]  // This is map in groovy (dictionary in python) . buildsarray is a list which contains dictionaries

    for (stage in ["one", "two", "three"]) {
      n = "$stage $os"
      stages[n] = eachStage(n)
    }
    buildsarray.add(stages)
    println stages
  }
  println buildsarray
  // Build array oputput is 
  //[{one 0=org.jenkinsci.plugins.workflow.cps.CpsClosure2@b07a01d, two 0=org.jenkinsci.plugins.workflow.cps.CpsClosure2@62af765, three 0=org.jenkinsci.plugins.workflow.cps.CpsClosure2@14e5a6de}, {one 1=org.jenkinsci.plugins.workflow.cps.CpsClosure2@5eb87ef7, two 1=org.jenkinsci.plugins.workflow.cps.CpsClosure2@164dc5cf, three 1=org.jenkinsci.plugins.workflow.cps.CpsClosure2@36cead20}, {one 2=org.jenkinsci.plugins.workflow.cps.CpsClosure2@28102bd2, two 2=org.jenkinsci.plugins.workflow.cps.CpsClosure2@62879acb, three 2=org.jenkinsci.plugins.workflow.cps.CpsClosure2@753b5fa4}, {one 3=org.jenkinsci.plugins.workflow.cps.CpsClosure2@40ebec57, two 3=org.jenkinsci.plugins.workflow.cps.CpsClosure2@7e51a073, three 3=org.jenkinsci.plugins.workflow.cps.CpsClosure2@40c7c7e7}]
  
  return buildsarray
}
def eachStage(String stagename) {

  return {
    stage("Inspection from fun $stagename") {}
  }
}

// Parameters for the build
  properties([parameters([
    string(name: 'BRANCH_NAME', defaultValue: env.BRANCH_NAME, description: 'Branch to build'),
    string(name: 'builddir', defaultValue: 'cookbook-openshift3-test-' + env.BUILD_NUMBER, description: 'Build directory'),
    string(name: 'nodename', defaultValue: 'cage', description: 'Node to build on'),
    string(name: 'CHEF_VERSION', defaultValue: '12.16.42-1', description: 'Chef version to use, eg 12.4.1-1'),
    string(name: 'OSE_VERSIONS', defaultValue: '1.3 1.4 1.5', description: 'OSE versions to build, separated by spaces'),
    string(name: 'CHEF_IPTABLES_COOKBOOK_VERSION', defaultValue: 'latest', description: 'iptables cookbook version, eg 1.0.0'),
    string(name: 'CHEF_SELINUX_COOKBOOK_VERSION', defaultValue: 'latest', description: 'selinux cookbook version, eg 0.7.2'),
    string(name: 'CHEF_YUM_COOKBOOK_VERSION', defaultValue: 'latest', description: 'yum cookbook version, eg 3.6.1'),
    string(name: 'CHEF_COMPAT_RESOURCE_COOKBOOK_VERSION', defaultValue: 'latest', description: 'compat_resource cookbook version'),
    string(name: 'CHEF_INJECT_COMPAT_RESOURCE_COOKBOOK_VERSION', defaultValue: 'false', description: 'whether to inject compat_resource cookbook version (eg true for some envs)'),
    booleanParam(name: 'dokitchen', defaultValue: true, description: 'Whether to run kitchen tests'),
    booleanParam(name: 'doshutit', defaultValue: true, description: 'Whether to run shutit tests')
    ]) ])
