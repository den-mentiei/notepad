# Design

Architecture, API design, etc.

## Papers and articles

- [cbloom on push/restore](http://cbloomrants.blogspot.com/2012/05/05-27-12-prefer-push-restore-to-push.html)
  ```cpp
	struct Scope { Scope *next; Data data; };

	Scope *top = 0;

	Scope::Scope(...): data(...) { next = top; top = this; }
	Scope::~Scope() { top = next; }
  ```
  vs
  ```cpp
	int Profile_Push() {
		int i = s_profile_vec.size();
		s_profile_vec.push_back();
		s_profile_vec.back().start = tick();
		return i;
	}

	void Profile_Restore(int i) {
		s_profile_vec[i].end = tick();
		// [start,end] for this block is now set, as is stack depth, store it somewhere permanent
		s_profile_vec.resize(i);
	}

	void Func1() {
		int p = Profile_Push();

		// ... other stuff ..
		Func2()

		Profile_Restore(p);
	}

  ```
