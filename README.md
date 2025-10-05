

graph family_tree {
    node person {
        has name: str;
    }

    edge parent_of;
    edge sibling_of;

    walker show_children {
        can visit person {
            print("Children of ", here.name, ":");
            for child in here.out_nodes("parent_of") {
                print(" - ", child.name);
            }
        }
    }

    walker show_siblings {
        can visit person {
            print("Siblings of ", here.name, ":");
            for sibling in here.out_nodes("sibling_of") {
                print(" - ", sibling.name);
            }
        }
    }
}

with entry {
    spawn {
        // Create people
        Hawah = spawn person(name="Hawah");
        Rashid = spawn person(name="Rashid");
        Adam= spawn person(name="Adam");

        // Define relationships
        Hawah -parent_of-> Rashid;
        Hawah -parent_of-> Adam;
        Rashid -sibling_of-> Adam;

        // Run walkers
        Rashid ++> show_siblings;
        Hawah ++> show_children;
    }
}

