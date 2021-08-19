## Ginkgo Test Suites

All Ginkgo Integration test suites are located in `test/integration` directory.

### Prerequisite
- Have all Nitro Based Instances in your EKS Cluster.
- Have at least 3 X c5.xlarge instance type or larger in terms of number of ENI/IP allocatable.

### Available Ginkgo Focus

The Integration test suite provides the following focuses.

- **[LOCAL]** 
   
  These tests can be run when the controller is running on Data Plane. This is idle for CI Setup where the controller runs on Data Plane instead of Control Plane. See `scripts/test/README.md` for details.
  ```
  # Use when running test on the Controller on EKS Control Plane. 
  --skip=LOCAL 
  ```
  
### How to Invoke the Ginkgo Tests

1. Set the environment variables.
   ```
   CLUSTER_NAME=<test-cluster-name>
   KUBE_CONFIG_PATH=<path-to-kube-config>
   AWS_REGION=<test-cluster-region>
   VPC_ID=<cluster-vpc-id>
   OS=<darwin/linux/etc>
   ```
2. Invoke all available test suites.
   ```
   cd integration
   echo "Running Validation Webhook Tests"
   (cd webhook && CGO_ENABLED=0 GOOS=$OS ginkgo -v -timeout 40m -- -cluster-kubeconfig=$KUBE_CONFIG_PATH -cluster-name=$CLUSTER_NAME --aws-region=$AWS_REGION --aws-vpc-id $VPC_ID)
   echo "Running Security Group for Pods Integration Tests"
   (cd perpodsg && CGO_ENABLED=0 GOOS=$OS ginkgo -v -timeout 40m -- -cluster-kubeconfig=$KUBE_CONFIG_PATH -cluster-name=$CLUSTER_NAME --aws-region=$AWS_REGION --aws-vpc-id $VPC_ID)
   ```

### Future Work
- Once we have more test suites, we can provide a script instead of invoking each suite manually.
- Add Windows tests to the list once the support is enabled.
- Move the script based tests in `integration-test` to Ginkgo Based integration/e2e test.