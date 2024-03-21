Anchoring
=========

Then anchoring nodes handles the integration of Digital Twin**s** data into the ontology of the system. It is then used to generate the 'problem' file for task planner for instance.



Digital-Twin Integrator
----------------------

This node handles the update of the ontology:
```mermaid
sequenceDiagram
    participant PT as Periodic Trigger
    participant OM as Ontology Manager
    participant DTI as Digital-Twin Integrator
    participant DT as Digital Twin

    DTI -->> DTI: lifecycle:Configure

    activate DTI
    DTI ->> DT: connect
    activate DT
    DT ->> DTI: true
    deactivate DT
    deactivate DTI

    loop Every Xs
        PT ->> DTI: trigger: act:update_state
        activate DTI

        DTI ->> OM: serv:get_ontology
        activate OM
        OM -->> DTI: ontology + process:result
        deactivate OM

        DTI -->> DTI: Compute list of entities

        DTI ->> DT: getData
        activate DT
        DT -->> DTI: data
        deactivate DT

        DTI ->> OM: act:validate_set_ontology
        activate OM
        OM -->> DTI: process:result
        deactivate OM

        loop While 'compute' functions
            DTI ->> DT: serv:compute
            activate DT
            DT -->> DTI: result
            deactivate DT

            DTI ->> OM: act:validate_set_ontology
            activate OM
            OM -->> DTI: process:result
            deactivate OM
        end

        DTI -->> PT: done
        deactivate DTI
        
    end
```

### Action server

The Digital-Twin Integrator provides the following action server:

`anchoring/digital_twin_integrator/update_state` which starts the process:
* Request:
  * string: knowledge_domain - (WIP) Selects the target ontology to update, is forwarded to the Ontology Manager. Currently not used.
* Feedback:
  * process:progress: Generic progress message. Currently implemented as a std_msgs/String with the name of the step: [listing_entities, getting_digital_twin_data, triggering_digital_twin_functions].
* Response:
  * process:result: Generic result message. Currently implemented as:
    * bool: success.
    * string: message.



Symbolic-Predicates Reader
-------------------------

The Symbolic-Predicates Reader (SPR) can be requested to transform the ontology into symbolic predicates in a target language. Different transformation libraries allow to export to different formats (e.g. PDDL, ...).

```mermaid
sequenceDiagram
    participant PT as Periodic Trigger
    participant OM as Ontology Manager
    participant DTI as Digital-Twin Integrator
    participant SPR as Symbolic-Predicate Reader

    loop Every Ys

        PT ->> DTI: trigger: act:update_state

        Note over DTI: Integrator process, see above

        DTI -->> PT: done

        PT ->> SPR: trigger: act:get_predicates
        activate SPR

        SPR ->> OM: serv:get_ontology
        activate OM
        OM -->> SPR: ontology + process:result
        deactivate OM

        SPR -->> SPR: Transform into target language (e.g. PDDL, ...)

        SPR -->> PT: done
        deactivate SPR
    end
```

### Action server

`planning/get_predicates` which retrieves the current state from the ontology and computes the predicates:
* Request:
  * string: knowledge_domain - (WIP) Selects the target ontology to retrieve from, is forwarded to the Ontology Manager. Currently not used.
* Feedback:
  * process:progress: Generic progress message. Currently implemented as a std_msgs/String with the name of the step: [].
* Response:
  * process:result: Generic result message. Currently implemented as:
    * bool: success.
    * string: message.