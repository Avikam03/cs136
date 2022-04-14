void remove_bstnode(int key, struct dictionary *d) {
    struct bstnode *prev = NULL;
    struct bstnode *curr = d->root;
    bool found = true;
    while (1) {
        if (curr->value > key) {
            if (curr->left) {
                prev = curr;
                curr = curr->left;
            } else {
                found = false;
                break;
            }
        } else if (curr->value < key) {
            if (curr->right) {
                prev = curr;
                curr = curr->right;
            } else {
                found = false;
                break;
            }
        } else {
            break;
        }
    }

    if (found) {
        if (curr->right && curr->left) {
            struct bstnode *newprev = NULL;
            struct bstnode *newcurr = curr->right;
            while (newcurr) {
                newprev = newcurr;
                newcurr = newcurr->left;
            }

            if (newcurr->right) {
                newprev->left = newcurr->right;
            } else {
                newprev->left = NULL;
            }

            if (prev->item < newcurr->item) {
                prev->right = newcurr;
            } else {
                prev->left = newcurr;
            }
            newcurr->right = curr->right;
            newcurr->left = curr->left;

            } else if (curr->left) {
                if (prev->item < curr->item) {
                    prev->right = curr->left;
                } else {
                    prev->left = curr->left;
                }
            } else if (curr->right) {
                if (prev->item < curr->item) {
                    prev->right = curr->right;
                } else {
                    prev->left = curr->right;
                }
            } else {
                if (prev->item < curr->item) {
                    prev->right = NULL;
                } else {
                    prev->left = NULL;
                }
            }
        free(curr->value);
        free(curr);
    }
    // else, we do nothing since the element was not found
}
