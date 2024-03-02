name: SonarCloud Analysis

on:
  push:
    branches:
      - main

jobs:
  sonarcloud:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          distribution: 'adopt'
          java-version: '11'

      - name: Build with Maven
        run: mvn clean install -B

      - name: SonarCloud Scan
        run: |
          mvn sonar:sonar \
            -Dsonar.organization=$SONAR_ORGANIZATION \
            -Dsonar.projectKey=$GITHUB_REPOSITORY \
            -Dsonar.host.url=https://sonarcloud.io \
            -Dsonar.login=$SONAR_TOKEN
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          
