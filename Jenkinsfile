@Library('releng-pipeline') _

hugo (
  appName: 'threadx-rtos-docs',
  build: [
    containerImage: 'eclipsefdn/hugo-node:h0.110.0-n18.13.0',
  ],
  deployment: [
    locationPath: '/threadx/docs'
  ]
)