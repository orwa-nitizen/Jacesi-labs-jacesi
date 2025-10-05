
graph family_tree {
    node person {
        has name: str;
    }

    edge parent_of;
    edge sibling_of;

    walker show_children {
        can visit person {
            let kids = here.out_nodes("parent_of");
            if len(kids) == 0 {
                print(here.name, "has no children.");
            } else {
                print("Children of", here.name, ":");
                for child in kids {
                    print(" -", child.name);
                }
            }
        }
    }

    walker show_siblings {
        can visit person {
            let sibs = here.out_nodes("sibling_of");
            if len(sibs) == 0 {
                print(here.name, "has no siblings.");
            } else {
                print("Siblings of", here.name, ":");
                for sibling in sibs {
                    print(" -", sibling.name);
                }
            }
        }
    }

    walker show_parents {
        can visit person {
            let parents = here.in_nodes("parent_of");
            if len(parents) == 0 {
                print(here.name, "has no parents recorded.");
            } else {
                print("Parents of", here.name, ":");
                for p in parents {
                    print(" -", p.name);
                }
            }
        }
    }
}

with entry {
    spawn {
        // Create people
        Hawah  = spawn person(name="Hawah");
        Rashid = spawn person(name="Rashid");
        Adam   = spawn person(name="Adam");

        // Define relationships
        Hawah -parent_of-> Rashid;
        Hawah -parent_of-> Adam;

        // Define siblings (bidirectional)
        Rashid -sibling_of-> Adam;
        Adam   -sibling_of-> Rashid;

        // Run walkers
        Rashid ++> show_siblings;
        Hawah  ++> show_children;
        Adam   ++> show_parents;
    }
}


