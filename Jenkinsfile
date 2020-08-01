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
    parallel(staticTests)

  }

  builds = allBuilds()

  for (stages in builds) {
    parallel(stages)
  }

}
def allBuilds() {
  buildsarray = []
  for (i = 0; i < 4; i++) {
    stages = [: ]

    for (stage in ["one", "two", "three"]) {
      n = "$stage $i"
      stages[n] = eachStage(n)
    }
    buildsarray.add(stages)
  }
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
