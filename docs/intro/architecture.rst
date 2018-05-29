Architecture
============


Infrastructure
^^^^^^^^^^^^^^

HelseID is currently exposed both on the internet and helsenettet in separate webfarms, fronted by separate load balancers.

Data is stored in a SQL Server failover cluster.

All hardware runs in the NHN datacenters.

Environments
^^^^^^^^^^^^

The following environments are available for development, test and production.


    Development
        https://helseid-sts.utvikling.nhn.no

        Unstable environment where new versions of HelseID is deployed continously.

    Test
        https://helseid-sts.test.nhn.no
        
        Test environment for external applications. More stable that development.

    QA
        https://helseid-sts.qa.nhn.no
        
        Production-like environment for quality assurance.

    Production
        https://helseid-sts.nhn.no 

        Our production environment


Metadata for the various environments can be found by navigating to one the urls above and clicking the "Metadata" link. The metadata contain information such as endpoints and signing key material.

For applications and APIs using HelseID, we recommend hosting them in an environment where dynamically accessing the metadata is possible. Most OIDC libraries support this.



